LABORATÓRIO DE SISTEMAS OPERACIONAIS - PTHREADS PARTE 2

Lucas Eleutério da Silva - TIA 32240597

Renan Pires de Miranda - TIA 32248253

Execução da instância AWS e instalação do git no terminal:
![image](https://github.com/brunoroveri/lab3parte2/assets/142548195/12ca5852-c429-4912-9c0f-5fbdd138716f)

Geração da chave SSH e clonagem do repositório:
![image](https://github.com/brunoroveri/lab3parte2/assets/142548195/2ac7ccca-96dd-49f3-a5e3-7f1981f4c73d)

Implementação sem Mutex:
![image](https://github.com/brunoroveri/lab3parte2/assets/142548195/f15782f3-a128-44f2-b7bf-0517cd9d598f)
![image](https://github.com/brunoroveri/lab3parte2/assets/142548195/61f53f02-3fc2-4d8d-8072-0c678a4c8c3d)

Implementação com Mutex:
![image](https://github.com/brunoroveri/lab3parte2/assets/142548195/607b5f01-7177-4c62-b126-e7eca64319a5)
![image](https://github.com/brunoroveri/lab3parte2/assets/142548195/3ff5b2b3-7383-4c07-9190-fea1f18c1c1a)

Envio dos arquivos criados para staging area, realização do commit e push ao repositório:
![image](https://github.com/brunoroveri/lab3parte2/assets/142548195/99e1ea1a-a4a2-43a4-a280-652e7186c546)

Instruções para compilação e execução do código:

Compilação:
Use o compilador gcc para compilar o código. O código utiliza a biblioteca pthread para threads, então é necessário incluir a flag -lpthread:

gcc nome_do_arquivo.c -o nome_do_programa -lpthread


Execução:
Para executar o programa, basta especificar a rota para o arquivo a ser executado. Por exemplo:

./nome_do_arquivo


Demonstração do código em funcionamento:

Implementação sem Mutex:
![image](https://github.com/brunoroveri/lab3parte2/assets/142548195/bcba69ac-38f3-4a41-9e27-66f463b9bf87)

Implementação com Mutex:
![image](https://github.com/brunoroveri/lab3parte2/assets/142548195/72a63370-9860-4f2b-9e9c-5f342108d1ea)


Diferenças entre as Implementações sem e com Mutex:
A diferença fundamental entre as implementações sem e com mutex está na forma como as threads interagem com a variável de soma (sum) que acumula a estimativa de π. Abaixo está uma análise detalhada das duas abordagens:

Implementação sem Mutex:
Isolamento de cálculo: Cada thread calcula uma parte da série de forma independente e armazena seu resultado em uma variável local. Ao final de seu cálculo, cada thread armazena sua soma parcial em um array global partial_sum indexado pelo seu rank.

Ausência de contenção: Como cada thread tem sua própria seção do array partial_sum, não há risco de duas ou mais threads tentarem atualizar o mesmo local de memória ao mesmo tempo. Isso elimina a necessidade de sincronização e pode levar a uma execução mais rápida em comparação com a implementação com mutex, dependendo do cenário.

Combinação de resultados: Após todas as threads terem completado sua execução, a thread principal (normalmente a thread 0) combina os resultados parciais armazenados no array partial_sum para obter a estimativa final de π.


Implementação com Mutex:
Soma Global Compartilhada: Existe uma variável global sum que todas as threads atualizam simultaneamente. Como várias threads podem tentar atualizar esta variável ao mesmo tempo, há um risco de condições de corrida.

Sincronização com Mutex: Para evitar condições de corrida, a atualização da variável global sum é protegida por um mutex. Quando uma thread deseja adicionar à sum, ela bloqueia o mutex, realiza a adição e, em seguida, libera o mutex. Isso garante que apenas uma thread possa atualizar a sum de cada vez.

Contenção e Sobrecarga: Embora o mutex evite condições de corrida, ele introduz contenção. Sempre que uma thread bloqueia o mutex, outras threads que desejam atualizar a sum devem esperar. Além disso, o bloqueio e desbloqueio do mutex têm sua própria sobrecarga. Isso pode tornar a implementação com mutex mais lenta, especialmente quando o número de threads é alto e/ou os cálculos realizados entre atualizações são relativamente pequenos.

Conclusão:
Muito embora não tenha havido divergência entre os resultados, a versão sem mutex é geralmente mais rápida porque evita a sobrecarga e contenção associadas ao uso de mutex. No entanto, ela requer uma estrutura de dados adicional (o array partial_sum) e uma etapa adicional para combinar os resultados parciais.
