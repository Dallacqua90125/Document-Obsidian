- Inteiro, float, string
```
int provas = 5;

float nota1 = 5.3f;
float nota2 = 6.8f;
float nota3 = 8.6f;

float media = (nota1 + nota2 + nota3) / 3;
Console.WriteLine(media);

string Nome = "Yan";
Console.WriteLine(Nome);
```

## BubbleSort
```
int[] array1 = {78,5,7,0};
int[] array2 = {3,10,40,150};
int arrayBraba = 0;
int arrayBraba2 = 0;
int resultPast = 100;
int result;

for (int i = 0; i < array1.Length; i++)
{
	for (int j = 0; j < array2.Length; j++)
	{
		result = array1[i] - array2[j];
		if(result < 0){
			result = result / (-1);
		}
		if(result < resultPast){
			resultPast = result;
			arrayBraba = array1[i];
			arrayBraba2 = array2[j];
		}

	}
}
Console.WriteLine( resultPast );
Console.WriteLine( arrayBraba );
Console.WriteLine( arrayBraba2 );

```