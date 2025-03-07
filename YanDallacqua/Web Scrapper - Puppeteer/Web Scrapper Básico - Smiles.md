- index.ts
````
import puppeteer from "puppeteer"; // importa o puppetter

import { saveToDatabase } from "./database"; // método de salvar no banco

  

export interface FlightData {  // interface para o banco

  date?: string;

  company?: string;

  seat?: string;

  origin: { iata?: string; time?: string; city?: string };

  destination: { iata?: string; time?: string; city?: string };

  miles?: string;

  price?: string;

}

  

const buildUrl = (origin: string, destination: string, date: number) =>

  `https://www.smiles.com.br/mfe/emissao-passagem/?adults=1&cabin=ALL&children=0&departureDate=${date}&infants=0&isElegible=false&isFlexibleDateChecked=false&returnDate=&searchType=g3&segments=1&tripType=2&originAirport=${origin}&destinationAirport=${destination}&novo-resultado-voos=true`;

  

const getDateRange = (start: string, end: string) => {

  let dates: string[] = [], current = new Date(start);

  while (current <= new Date(end)) {

    dates.push(current.toISOString().split('T')[0]);

    current.setDate(current.getDate() + 1);

  }

  return dates;

};

  

(async () => {

  const browser = await puppeteer.launch({ headless: true,

    executablePath: puppeteer.executablePath(),

    args: ['--no-sandbox', '--disable-setuid-sandbox']});

  
  

  const page = await browser.newPage();

  

  const origin = 'GRU', destination = 'MIA';

  const dateRange = getDateRange("2025-01-31", "2025-02-04");

  

  const clickShowMore = async () => {

    while (await page.$('#SelectFlightList-ida-more')) {

      await page.evaluate(() => window.scrollBy(0, window.innerHeight));

      await page.click('#SelectFlightList-ida-more').catch(() => {});

      console.log("Botão ver mais clicado");

    }

  };

  

  const clickOutrasCompanhias = async () => {

    const outroCompanhias = await page.$('#SelectFlightHeader-ida-button-congener');

    if (outroCompanhias) {

      await outroCompanhias.click();

      console.log("Botão 'outras companhias' clicado");

    } else {

      console.log("Botão 'outras companhias' não encontrado.");

    }

  };

  

  const getFlights = async (date: string) => {

    return await page.evaluate((date) => {

      const [year, month, day] = date.split('-');

      const formattedDate = `${day}/${month}/${year}`;

      return Array.from(document.querySelectorAll('.header-content')).map(flight => ({

        date: formattedDate,

        company: flight.querySelector('.company')?.textContent?.trim(),

        seat: flight.querySelector('.seat')?.textContent?.trim(),

        origin: {

          iata: flight.querySelectorAll('.iata-code')[0]?.textContent?.split(' ')[0]?.trim(),

          time: flight.querySelectorAll('.iata-code')[0]?.querySelector('strong')?.textContent?.trim(),

          city: flight.querySelectorAll('.city')[0]?.textContent?.trim(),

        },

        destination: {

          iata: flight.querySelectorAll('.iata-code')[1]?.textContent?.split(' ')[0]?.trim(),

          time: flight.querySelectorAll('.iata-code')[1]?.querySelector('strong')?.textContent?.trim(),

          city: flight.querySelectorAll('.city')[1]?.textContent?.trim(),

        },

        miles: flight.querySelector('.price .miles strong')?.textContent?.trim(),

        price: flight.querySelector('.price .miles')?.textContent?.match(/R\$\s*([\d.,]+)/)?.[0]?.trim(),

      }));

    }, date);

  };

  

  const filterEmptyFlights = (flights: FlightData[]) => {

    return flights.filter(flight => {

      const isOriginEmpty = !flight.origin.iata && !flight.origin.time && !flight.origin.city;

      const isDestinationEmpty = !flight.destination.iata && !flight.destination.time && !flight.destination.city;

      return !(isOriginEmpty && isDestinationEmpty);

    });

  };

  
  

  for (const date of dateRange) {

    console.log(`Buscando voos para ${date}`);

    await page.goto(buildUrl(origin, destination, new Date(`${date}T00:00:00`).getTime()), { waitUntil: 'networkidle2' });

    try {

      await page.waitForSelector('.flight-summary', { timeout: 15000 });

      await clickShowMore();

  

      let flights: FlightData[] = await getFlights(date);

      await clickOutrasCompanhias();

      await page.evaluate(() => new Promise(resolve => setTimeout(resolve, 10000)));

      await clickShowMore();

      const additionalFlights: FlightData[] = await getFlights(date);

      flights = flights.concat(additionalFlights);

      flights = filterEmptyFlights(flights);

      if (flights.length) {

        console.log(`Voos encontrados para ${date}:`, flights);

        await saveToDatabase(flights);

      } else {

        console.log(`Nenhum voo encontrado para ${date}.`);

      }

    } catch (error) {

      console.log(`Erro ao buscar voos para ${date}:`, error);

    }

  }

  

  await browser.close();

})();

````

- Database.ts
````
import { FlightData } from './index';

import { Client } from "pg";

  

// Configurações do banco de dados

const dbConfig = {

  user: process.env.DB_USER || 'postgres',

  host: process.env.DB_HOST || 'localhost',

  database: process.env.DB_NAME || 'postgres',

  password: process.env.DB_PASSWORD || 'postgres',

  port: parseInt(process.env.DB_PORT || '5432'),

};

  

// Função para salvar os dados no banco de dados

export async function saveToDatabase(flights: FlightData[]) {

  const client = new Client(dbConfig);

  

  try {

    await client.connect(); // Conectar ao banco de dados

  

    for (const flight of flights) {

      await client.query(

        `INSERT INTO flights (date, company, seat, origin_iata, origin_time, origin_city, destination_iata, destination_time, destination_city, miles, price)

         VALUES ($1, $2, $3, $4, $5, $6, $7, $8, $9, $10, $11)`,

        [

          flight.date,

          flight.company,

          flight.seat,

          flight.origin.iata,

          flight.origin.time,

          flight.origin.city,

          flight.destination.iata,

          flight.destination.time,

          flight.destination.city,

          flight.miles,

          flight.price,

        ]

      );

    }

  

    console.log('Dados salvos no banco de dados com sucesso!');

  } catch (error) {

    console.error('Erro ao salvar dados no banco de dados:', error);

  } finally {

    await client.end(); // Fechar a conexão com o banco de dados

  }

}
````
