#include <string.h>
#include <stdio.h>
#include <windows.h>
#include <locale.h>
#include <time.h>
// preco,ano,marca,modelo,condicao,combustivel,odometro,status,cambio,tamanho,tipo,cor
//  Definindo tamanho máximo para as strings
#define MAX 1000

// Definindo tipos de dados apropriados para as strings
typedef char String[MAX];

struct Veiculo
{
    int preco;
    int ano;
    String marca;
    String modelo;
    char condicao;
    char combustivel;
    String odometro;
    char status;
    char cambio;
    char tamanho;
    char tipo;
    String cor;
};

struct Marca
{
    char nome;
};
struct Veiculo carro;

void veiculos_oferta()
{

    setlocale(LC_ALL, "Portuguese");
    FILE *arquivo = fopen("veiculos_oferta.txt", "r");
    if (arquivo == NULL)
    {
        perror("veiculos_oferta.csv");
    }

    else
    {
        printf("arquivo aberto com sucesso");
    }

    return 0;
}
void venda_veiculos()
{

    setlocale(LC_ALL, "Portuguese");
    FILE *arquivo = fopen("veiculos_ofertas.csv", "r");
    FILE *arquivo2 = fopen("temp.csv", "w");
    FILE *vendas = fopen("veiculos_estoque.csv", "a");
    FILE *historico = fopen("historico_compras.csv", "a");
    char temp[MAX];
    int i = 0;
    time_t t = time(NULL);
    struct tm tm = *localtime(&t);
    if (arquivo == NULL)
    {
        perror("veiculos_oferta.csv");
    }

    else
    {
        char ler[MAX];
        int i = 0;
        printf("Digite as informações do veiculo a ser vendido.\n (preco,ano,modelo,condicao,combustivel,odometro,status,cambio,tamanho,tipo,cor)");
        scanf("%d,%d,%99[^\n],%99[^\n],%c,%c,%99[^\n],%c,%c,%c,%c,%99[^\n]",
              &carro.preco, &carro.ano, carro.marca, carro.modelo,
              &carro.condicao, &carro.combustivel, carro.odometro,
              &carro.status, &carro.cambio, &carro.tamanho,
              &carro.tipo, &carro.cor);

        while (fgets(ler, sizeof(ler), arquivo) != NULL)
        {
             
            if (carro.preco != atoi(ler) && carro.ano != atoi(ler) && strcmp(carro.marca, ler) != 0 && strcmp(carro.modelo, ler) != 0 && carro.condicao != ler &&
                carro.combustivel != ler && strcmp(carro.odometro, ler) != 0 && carro.status != ler &&
                carro.cambio != ler && carro.tamanho != ler &&
                carro.tipo != ler && strcmp(carro.cor, ler) != 0)
            {
                fprintf(arquivo2, "%s", ler);
            }
            // verifica se é diferente para copiar SOMENTE 1 VEZ para o documento de vendas e historico, sem isso vai copiar as mesmas informações até ficar do tamanho do arq original
             if (carro.preco != atoi(ler) && carro.ano != atoi(ler) && strcmp(carro.marca, ler) != 0 && strcmp(carro.modelo, ler) != 0 && carro.condicao != ler &&
                carro.combustivel != ler && strcmp(carro.odometro, ler) != 0 && carro.status != ler &&
                carro.cambio != ler && carro.tamanho != ler &&
                carro.tipo != ler && strcmp(carro.cor, ler) != 0)
            {
                fprintf(vendas, "%d,%d,%s,%s,%c,%c,%s,%c,%c,%c,%c,%s\n",
                    carro.preco, carro.ano, carro.marca, carro.modelo,
                    carro.condicao, carro.combustivel, carro.odometro, carro.status,
                    carro.cambio, carro.tamanho, carro.tipo, carro.cor);
                fprintf(historico, "%d/%02d/%02d %02d:%02d:%02d,", tm.tm_year + 1900, tm.tm_mon + 1, tm.tm_mday, tm.tm_hour, tm.tm_min, tm.tm_sec);
                fprintf(historico, "%d,%d,%s,%s,%c,%c,%s,%c,%c,%c,%c,%s\n",
                    carro.preco, carro.ano, carro.marca, carro.modelo,
                    carro.condicao, carro.combustivel, carro.odometro, carro.status,
                    carro.cambio, carro.tamanho, carro.tipo, carro.cor);
                    break;
            }
            
            
            i++;
        }

        fclose(vendas);
        fclose(arquivo);
        fclose(arquivo2);
        fclose(historico);
        remove("veiculos_ofertas.csv");
        rename("temp.csv", "veiculos_ofertas.csv");
    }
}
int main()
{
    int opcao;
    printf("Escolha uma opcao abaixo: \n [1] Abrir veiculos em ofertas.\n");
    scanf("%d", &opcao);
    if (opcao == 1)
    {
        venda_veiculos();
    }
    else
    {
        return 0;
    }

    return 0;
}
