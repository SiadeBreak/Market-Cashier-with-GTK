# Guia de Utilização

O Software foi desenvolvido utilizando a linguagem C e requer a instalação da biblioteca GTK para o funcionamento adequado das Interfaces Gráficas. É fundamental realizar todas as inserções de dados diretamente nas Interfaces Gráficas para garantir o funcionamento correto do programa. As Regras de Instalação da Biblioteca e as orientações para o uso da aplicação estão detalhadas a seguir.

<br>

## Regras de Instalação da Biblioteca GTK

1.	**Baixe o instalador MSYS2**: O instalador possibilita o download da Biblioteca de forma mais rápida e fácil. É encontrado no seguinte link: https://github.com/msys2/msys2-installer/releases/download/2023-07-18/msys2-x86_64-20230718.exe.

<br>

2.	**O Terminal do MSYS2 será aberto**: Copie e cole a seguinte linha no Terminal (sem as aspas): “ pacman -S mingw-w64-ucrt-x86_64-gcc ”. ATENÇÃO! Para colar, aperte o botão direito do mouse e clique em Paste. “Ctrl + C” cria um comando diferente no Terminal!
<br>

3.	**Após a instalação dessas ferramentas**: Copie e cole a seguinte linha no Terminal apenas para conferir se o MSYS2 está corretamente instalado (sem as aspas): “ gcc --version ”. O output deve ser o seguinte:

>*gcc.exe (Rev7, Built by MSYS2 project) 13.1.0*
>
>*Copyright (C) 2023 Free Software Foundation, Inc.*
>
>*This is free software; see the source for copying conditions.  There is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.*

<br>

4.	**Instale os componentes do GTK**: No mesmo terminal, copie e cole todos os seguintes códigos:

`*pacman -S mingw-w64-x86_64-gtk4*`

`*pacman -S mingw-w64-x86_64-gtk3*`

`*pacman -S mingw-w64-x86_64-toolchain base-devel*`

<br>

5.	**Instale o Code::Blocks**: Essa IDE facilita a configuração da biblioteca GTK. Para instalar, acesse o site oficial do Code::Blocks ou clique no seguinte link:
https://sourceforge.net/projects/codeblocks/files/Binaries/20.03/Windows/codeblocks-20.03-setup.exe/download

<br>

## Configurando o GTK no Code::Blocks

Ao abrir o Code::Blocks, selecione a opção de menu *Settings* > *Compiler...*

![image](https://github.com/SiadeBreak/Market-Cashier-with-GTK/assets/53356154/0bb1e71d-5f5f-49aa-be16-eb8b79c33817)

![image](https://github.com/SiadeBreak/Market-Cashier-with-GTK/assets/53356154/e705f9ad-1c83-4bc3-8573-8fc7dbd10120)

Nessa tela, clique em *Toolchain executables* e no campo *“Compiler’s installation directory”* informe o local de instalação do MSYS2:

![image](https://github.com/SiadeBreak/Market-Cashier-with-GTK/assets/53356154/b5e923b0-a197-4b45-965e-1d9b5ed56227)

Crie um novo projeto no Code::Blocks na opção de Menu *File* > *New* > *Project...*

![image](https://github.com/SiadeBreak/Market-Cashier-with-GTK/assets/53356154/9ddb2df8-a33b-47e3-9034-713107287a76)

Selecione a opção *Empty Project*:

![image](https://github.com/SiadeBreak/Market-Cashier-with-GTK/assets/53356154/8103807a-4903-47df-bdfd-ba83c8d402e4)

Nomeie o arquivo como quiser. Ele será necessário apenas para configurar o ambiente para a biblioteca GTK.
Retorne à opção de Menu *File* > *New* e selecione > *Empty File*. Adicione-a ao arquivo de Projeto criado anteriormente.

![image](https://github.com/SiadeBreak/Market-Cashier-with-GTK/assets/53356154/406cd068-5ca9-4307-8811-934c696265d4)

Agora é necessário informar o caminho das bibliotecas GTK. Na opção de Menu *Project*, selecione > *Build Options...*

![image](https://github.com/SiadeBreak/Market-Cashier-with-GTK/assets/53356154/dc03eda2-212c-4d9e-ba95-5670f62eb7f3)


Na seguinte tela, selecione a aba *Other compiler options* e cole a seguinte linha (Tanto em Debug, quanto em Release!): `pkg-config --cflags gtk+-3.0`

![image](https://github.com/SiadeBreak/Market-Cashier-with-GTK/assets/53356154/c7f1e077-3d34-4af0-9f48-acde6522b4c4)

![image](https://github.com/SiadeBreak/Market-Cashier-with-GTK/assets/53356154/4df1c14a-fc87-4933-9a07-84c11c276130)

Por fim, na aba *Linker settings*, cole a seguinte linha no campo *Other linker options*: `pkg-config --libs gtk+-3.0`

![image](https://github.com/SiadeBreak/Market-Cashier-with-GTK/assets/53356154/d92d3ad8-6ea9-4a06-acb6-b62be16f5c4b)

![image](https://github.com/SiadeBreak/Market-Cashier-with-GTK/assets/53356154/9357f794-fbac-48ab-ab27-7aedc755a3e8)

Pronto! Sua IDE e sua biblioteca GTK estão configuradas! Copie e cole todo o código do arquivo .txt **Código do Workshop Final** no Empty File criado e clique em Build and run.

![image](https://github.com/SiadeBreak/Market-Cashier-with-GTK/assets/53356154/ad924194-4374-4cf7-86db-dd7691880d87)

<br>

## Referências
https://www.gtk.org/docs/installations/windows/#using-gtk-from-msys2-packages

<br>

https://www.treinaweb.com.br/blog/criando-uma-aplicacao-c-com-gtk-no-codeblocks/

<br>

## Orientações para o uso da aplicação

1.	**Comece pelo Estoque**: Só é possível comprar produtos cadastrados! Na aba Gerente é possível cadastrar quais produtos serão vendidos. Informe o código da maneira que preferir. No preço, use Vírgula ( , ) para informar as casas decimais. Evite escrever caracteres além do necessário. Ex.: Para cadastrar um produto de R$3,99, apenas escreva 3,99 na aba Price.

<br>

2.	**Evite usar “ç” ou “~”**: Apesar de não apresentar qualquer erro ao usá-los, o programa não os imprime corretamente, afetando a aparência.

<br>

3.	**Nenhuma janela é fechada automaticamente**: Ao clicar em qualquer ponto da tela enquanto uma janela está aberta, ela será minimizada, permanecendo à espera. Evite abrir uma nova janela antes de terminar as ações na janela atual. Isso pode causar erro no programa.

<br>

4.	**Use o código para selecionar produtos**: Devido à falta de Códigos de Barras, os produtos devem ser adicionados por meio do código cadastrado no Estoque. Clique na opção Adicionar dentro da Lista de Compras e adicione quantos desejar.

<br>

5.	**Pagamento**: Não há nenhuma forma de Pagamento cadastrada. Fica a critério do cliente adicioná-las. Ao adicionar, surgirá um botão com o nome dessa forma de pagamento. Clique nela e escolha o valor a ser pago. 

<br>

6.	**Nota Fiscal**: Para encerrar a compra, clique na aba Gerar Nota Fiscal, seu cupom será impresso no Terminal do programa que está sendo executado. Mas lembre-se: só funciona ao pagar por todos os produtos!
