#include <stdio.h> #include <stdlib.h> #include <string.h> #include <locale.h>

#define MAX_ITEMS 50 #define MAX_CART_ITEMS 50

typedef struct { char descricao[30]; valor flutuante; id int; } Item;

Item catalogo[MAX_ITEMS]; int totalItens = 0;

typedef struct { Item item; int qtd; } ItemCarrinho;

ItemCarrinho carrinhoCompras[MAX_CART_ITEMS]; int totalCarrinho = 0;

Item buscarItemPorId(int id) { Item itemInexistente = {"", 0.0, -1}; for (int i = 0; i < totalItens; i++) { if (catalogo[i].id == id) { return catalogo[i]; } } return itemInexistente; }

void exibirDetalhesItem(Item item) { printf("ID: %d\n", item.id); printf("Descrição: %s\n", item.descricao); printf("Valor: R$ %.2f\n", item.valor); printf("--------------------------\n"); }

int verificarCarrinho(int id) { for (int i = 0; i < totalCarrinho; i++) { if (carrinhoCompras[i].item.id == id) { retorno i; } } retorno -1; }

void adicionarItem() { if (totalItens >= MAX_ITEMS) { puts("Limite de itens no catálogo atingido.\n"); retornar; }

Item novoItem;
novoItem.id = totalItens + 1;

printf("Digite a descrição do item: ");
scanf(" %29[^\n]", novoItem.descricao);
while (getchar() != '\n');

printf("\nDigite o valor do item: ");
scanf("%f", &novoItem.valor);
while (getchar() != '\n');

printf("Item cadastrado com sucesso!\n");
catalogo[totalItens] = novoItem;
totalItens++;
}

void listarItens() { if (totalItens == 0) { puts("Nenhum item no catálogo!\n"); retornar; }

puts("Itens cadastrados:\n");
for (int i = 0; i < totalItens; i++) {
    exibirDetalhesItem(catalogo[i]);
}
}

void adicionarAoCarrinho() { int id, quantidade;

if (totalCarrinho >= MAX_CART_ITEMS) {
    puts("Carrinho cheio.\n");
    return;
}

if (totalItens == 0) {
    puts("Nenhum item cadastrado.\n");
    return;
}

listarItens();

printf("Digite o ID do item que deseja adicionar ao carrinho: ");
scanf("%d", &id);

Item itemSelecionado = buscarItemPorId(id);
if (itemSelecionado.id == -1) {
    printf("Erro: Item com ID %d não encontrado!\n", id);
    return;
}

printf("Digite a quantidade: ");
scanf("%d", &quantidade);

int indice = verificarCarrinho(id);

if (indice != -1) {
    carrinhoCompras[indice].qtd += quantidade;
    printf("Quantidade do item '%s' atualizada!\n", carrinhoCompras[indice].item.descricao);
} else {
    carrinhoCompras[totalCarrinho].item = itemSelecionado;
    carrinhoCompras[totalCarrinho].qtd = quantidade;
    totalCarrinho++;
    puts("Item adicionado ao carrinho.\n");
}
}

void visualizarCarrinho() { if (totalCarrinho == 0) { puts("Carrinho vazio.\n"); retornar; }

for (int i = 0; i < totalCarrinho; i++) {
    printf("ID: %d\n", carrinhoCompras[i].item.id);
    printf("Descrição: %s\n", carrinhoCompras[i].item.descricao);
    printf("Quantidade: %d\n", carrinhoCompras[i].qtd);
    printf("Valor unitário: R$ %.2f\n", carrinhoCompras[i].item.valor);
    printf("Valor total: R$ %.2f\n", carrinhoCompras[i].item.valor * carrinhoCompras[i].qtd);
    printf("--------------------------\n");
}
}

void removerDoCarrinho() { id int;

if (totalCarrinho == 0) {
    puts("Carrinho já está vazio.\n");
    return;
}

visualizarCarrinho();

printf("Digite o ID do item que deseja remover: ");
scanf("%d", &id);

int indice = verificarCarrinho(id);
if (indice == -1) {
    printf("Erro: Item com ID %d não encontrado!\n", id);
    return;
}

for (int i = indice; i < totalCarrinho - 1; i++) {
    carrinhoCompras[i] = carrinhoCompras[i + 1];
}
totalCarrinho--;

puts("Item removido com sucesso.\n");
}

void finalizarCompra() { if (totalCarrinho == 0) { puts("Carrinho vazio, adicione itens antes de finalizar.\n"); retornar; }

float totalCompra = 0;
puts("-------- Resumo da compra --------\n");

for (int i = 0; i < totalCarrinho; i++) {
    printf("Descrição: %s\n", carrinhoCompras[i].item.descricao);
    printf("Quantidade: %d\n", carrinhoCompras[i].qtd);
    printf("Subtotal: R$ %.2f\n", carrinhoCompras[i].item.valor * carrinhoCompras[i].qtd);
    printf("--------------------------\n");

    totalCompra += carrinhoCompras[i].item.valor * carrinhoCompras[i].qtd;
}

printf("Valor total da compra: R$ %.2f\n", totalCompra);
puts("Obrigado pela compra!\n");

totalCarrinho = 0; 
}

void exibirMenu(int escolha) { do { sistema("cls");

    puts("----- BEM-VINDO AO MERCADO VIRTUAL -----\n");
    puts("1. Cadastrar item");
    puts("2. Listar itens");
    puts("3. Adicionar item ao carrinho");
    puts("4. Remover item do carrinho");
    puts("5. Visualizar carrinho");
    puts("6. Finalizar compra");
    puts("7. Sair\n");

    scanf("%d", &escolha);

    system("cls");

    switch (escolha) {
    case 1:
        adicionarItem();
        break;
    case 2:
        listarItens();
        break;
    case 3:
        adicionarAoCarrinho();
        break;
    case 4:
        removerDoCarrinho();
        break;
    case 5:
        visualizarCarrinho();
        break;
    case 6:
        finalizarCompra();
        break;
    case 7:
        printf("Saindo...\n");
        
            printf(".");
            
        }
        break;
    default:
        puts("Opção inválida!\n");
        break;
    }

    puts("\nPressione ENTER para continuar...");
    getchar();
    getchar();

} while (escolha != 7);
}

int main() { setlocale(LC_ALL, "português"); int opcao = 0; exibirMenu(opção); }
