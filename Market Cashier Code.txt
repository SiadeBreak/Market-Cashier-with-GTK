#include <gtk/gtk.h>
#include <stdlib.h>
#include <glib.h>

GtkWidget *principalJanela;

GtkWidget *textoArrumador;

GtkWidget *stackRight;
GtkWidget *stackGerente;

GtkWidget *janelaListaAdicionar;
GtkWidget *scrolledWindowListaBot;
GtkWidget *layoutScrolledWindowListaBot;
GtkWidget *layoutScrolledWindowListaBotProduto;
GtkWidget *layoutScrolledWindowListaBotQuantity;
GtkWidget *layoutScrolledWindowListaBotPrice;

GtkWidget *janelaPagamentoAdicionar;
GtkWidget *layoutScrolledWindowPagamentoBotLeftBot;
GtkWidget *layoutScrolledWindowPagamentoBotLeftBotCompleto[20];

GtkWidget *janelaGerenteAdicionar;
GtkWidget *scrolledWindowGerenteBot;
GtkWidget *layoutScrolledWindowGerenteBot;
GtkWidget *layoutScrolledWindowGerenteBotProduto;
GtkWidget *layoutScrolledWindowGerenteBotCode;
GtkWidget *layoutScrolledWindowGerenteBotPrice;

GtkWidget *janelaPagamentoQualquer;



GtkWidget *layoutTextoListaBot[100];
GtkWidget *labelTextoListaProduto[100];
gchar *nomesListaProduto[100];
GtkWidget *labelTextoListaQuantity[100];
gchar *nomesListaQuantity[100];
GtkWidget *labelTextoListaPrice[100];
double nomesListaPrice[100];

GtkWidget *layoutPagamentoBotRight;
GtkWidget *botaoPagamentoAdicionarNovo[20];
gchar *nomesPagamentoAdicionarNovoPagamento[20];
gchar *nomesPagamentoAdicionarNovoCode[20];

GtkWidget *textoPagamentoBotLeftTopPrice;
GtkWidget *labelTextoPagamentoNome[20];
GtkWidget *labelTextoPagamentoToBePaidPrice[20];
gchar *nomesPagamentoQualquerNome[20];
gchar *nomesPagamentoQualquerPrice[20];

GtkWidget *layoutTextoGerenteBot[100];
GtkWidget *labelTextoGerenteProduto[100];
gchar *nomesGerenteProduto[100];
GtkWidget *labelTextoGerenteCode[100];
gchar *nomesGerenteCode[100];
GtkWidget *labelTextoGerentePrice[100];
gchar *nomesGerentePrice[100];

GPtrArray *vetorDadosLista;
GPtrArray *vetorDadosPagamento;
GPtrArray *vetorDadosPagamentoQualquer;
GPtrArray *vetorDadosGerente;

GtkWidget *entryProduto;
GtkWidget *entryCode;
GtkWidget *entryPrice;
GtkWidget *entryQuantity;
GtkWidget *entryNomePagamento;
GtkWidget *entryToBePaidPrice;


int ContadorLista = 0;
gint buscaEstoque;
int ContadorPagamento = 0;
int ContadoPagamentoQualquer = 0;
gint buscaCode;
int ContadorGerente = 0;
int g = 0;
int j = 0;
int Salvador = 0;

double TotalPrice = 0;
double TotalToBePaidPrice = 0;



void botaoCancelarClicado(GtkButton *button, gpointer data) {
    GtkWidget *janela = GTK_WIDGET(data);
    gtk_window_close(GTK_WINDOW(janela));
}



void botaoListaClicado(GtkWidget *widget, gpointer data) {
    gtk_stack_set_visible_child_name(GTK_STACK(stackRight), "paginaLista");

}

void botaoListaAdicionarOKClicado(GtkWidget *widget, gpointer data){

    Salvador = 0;

    GtkWidget *entryCode = GTK_WIDGET(g_ptr_array_index(vetorDadosLista, 0));
    const gchar *textoCode = gtk_entry_get_text(GTK_ENTRY(entryCode));

    GtkWidget *entryQuantity = GTK_WIDGET(g_ptr_array_index(vetorDadosLista, 1));
    const gchar *textoQuantity = gtk_entry_get_text(GTK_ENTRY(entryQuantity));

    double Quantity = strtod(textoQuantity, NULL);



    if(Quantity < 0){
        Salvador = 1;
        GtkWidget *janelaErro = gtk_message_dialog_new(NULL, GTK_DIALOG_MODAL, GTK_MESSAGE_ERROR, GTK_BUTTONS_OK, "Quantidade deve ser maior que 0!");

        gtk_dialog_run(GTK_DIALOG(janelaErro));
        gtk_widget_destroy(janelaErro);
        gtk_widget_destroy(janelaListaAdicionar);
    }


    if(Salvador == 0){
        for(g = 0; g < 100; g++){
            if(textoCode && nomesGerenteCode[g]){
                buscaEstoque = g_ascii_strcasecmp(textoCode, nomesGerenteCode[g]);

                if(buscaEstoque == 0){
                    gtk_label_set_text(GTK_LABEL(labelTextoListaProduto[ContadorLista]), nomesGerenteProduto[g]);
                    nomesListaProduto[ContadorLista] = gtk_label_get_text(GTK_LABEL(labelTextoListaProduto[ContadorLista]));

                    gtk_label_set_text(GTK_LABEL(labelTextoListaQuantity[ContadorLista]), textoQuantity);
                    nomesListaQuantity[ContadorLista] = gtk_label_get_text(GTK_LABEL(labelTextoListaQuantity[ContadorLista]));

                    double Price = strtod(nomesGerentePrice[g], NULL);
                    nomesListaPrice[ContadorLista] = Quantity * Price;

                    gchar *arrumadorPrice = g_strdup_printf("R$%.2f", nomesListaPrice[ContadorLista]);
                    gtk_label_set_text(GTK_LABEL(labelTextoListaPrice[ContadorLista]), arrumadorPrice);

                    TotalPrice = TotalPrice + nomesListaPrice[ContadorLista];
                    TotalToBePaidPrice = TotalToBePaidPrice + nomesListaPrice[ContadorLista];

                    arrumadorPrice = g_strdup_printf("R$%.2f", TotalToBePaidPrice);
                    gtk_label_set_text(GTK_LABEL(textoPagamentoBotLeftTopPrice), arrumadorPrice);

                    ContadorLista = ContadorLista + 1;
                    g_free(arrumadorPrice);
                    break;
                }
            }
        }
        gtk_widget_destroy(janelaListaAdicionar);
    }
}

void botaoListaAdicionarClicado(GtkWidget *widget, gpointer data) {
    janelaListaAdicionar = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_default_size(GTK_WINDOW(janelaListaAdicionar), 400, 200);

    GtkWidget *layoutListaAdicionar;
    GtkWidget *layoutListaAdicionarTop;
    GtkWidget *layoutListaAdicionarMiddle;
    GtkWidget *layoutListaAdicionarBot;
    GtkWidget *layoutListaAdicionarArrumador;

    GtkWidget *layoutTextoEntryCode;
    GtkWidget *layoutTextoEntryQuantity;
    GtkWidget *textoEntryCode;
    GtkWidget *textoEntryQuantity;

    GtkWidget *botaoListaAdicionarOK;
    GtkWidget *botaoListaAdicionarCancel;



    layoutListaAdicionar = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(janelaListaAdicionar), layoutListaAdicionar);

    layoutListaAdicionarTop = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutListaAdicionar), layoutListaAdicionarTop);
    gtk_widget_set_hexpand(layoutListaAdicionarTop, TRUE);
    gtk_widget_set_vexpand(layoutListaAdicionarTop, TRUE);

    layoutListaAdicionarMiddle = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutListaAdicionar), layoutListaAdicionarMiddle);
    gtk_widget_set_hexpand(layoutListaAdicionarMiddle, TRUE);
    gtk_widget_set_vexpand(layoutListaAdicionarMiddle, TRUE);

    layoutListaAdicionarBot = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutListaAdicionar), layoutListaAdicionarBot);

    layoutListaAdicionarArrumador = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutListaAdicionar), layoutListaAdicionarArrumador);


    layoutTextoEntryCode = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 0);
    gtk_container_add(GTK_CONTAINER(layoutListaAdicionarTop), layoutTextoEntryCode);
    gtk_widget_set_hexpand(layoutTextoEntryCode, TRUE);

    layoutTextoEntryQuantity = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 0);
    gtk_container_add(GTK_CONTAINER(layoutListaAdicionarTop), layoutTextoEntryQuantity);
    gtk_widget_set_hexpand(layoutTextoEntryQuantity, TRUE);

    textoEntryCode = gtk_label_new("Code");
    gtk_container_add(GTK_CONTAINER(layoutTextoEntryCode), textoEntryCode);
    gtk_widget_set_hexpand(textoEntryCode, TRUE);

    textoEntryQuantity = gtk_label_new("Quantidade");
    gtk_container_add(GTK_CONTAINER(layoutTextoEntryQuantity), textoEntryQuantity);
    gtk_widget_set_hexpand(textoEntryQuantity, TRUE);


    textoArrumador = gtk_label_new("");
    gtk_container_add(GTK_CONTAINER(layoutListaAdicionarMiddle), textoArrumador);
    gtk_widget_set_hexpand(textoArrumador, TRUE);

    entryCode = gtk_entry_new();
    gtk_container_add(GTK_CONTAINER(layoutListaAdicionarMiddle), entryCode);
    gtk_widget_set_hexpand(entryCode, TRUE);
    gtk_widget_set_vexpand(entryCode, TRUE);

    entryQuantity = gtk_entry_new();
    gtk_container_add(GTK_CONTAINER(layoutListaAdicionarMiddle), entryQuantity);
    gtk_widget_set_hexpand(entryQuantity, TRUE);
    gtk_widget_set_vexpand(entryQuantity, TRUE);

    textoArrumador = gtk_label_new("");
    gtk_container_add(GTK_CONTAINER(layoutListaAdicionarMiddle), textoArrumador);
    gtk_widget_set_hexpand(textoArrumador, TRUE);


    vetorDadosLista = g_ptr_array_new();
    g_ptr_array_add(vetorDadosLista, entryCode);
    g_ptr_array_add(vetorDadosLista, entryQuantity);



    botaoListaAdicionarCancel = gtk_button_new_with_label("Cancelar");
    gtk_box_pack_end(GTK_BOX(layoutListaAdicionarBot), botaoListaAdicionarCancel, FALSE, FALSE, 0);

    botaoListaAdicionarOK = gtk_button_new_with_label("OK");
    gtk_box_pack_end(GTK_BOX(layoutListaAdicionarBot), botaoListaAdicionarOK, FALSE, FALSE, 0);

    g_signal_connect(G_OBJECT(botaoListaAdicionarOK), "clicked", G_CALLBACK(botaoListaAdicionarOKClicado), entryCode);
    g_signal_connect(G_OBJECT(botaoListaAdicionarCancel), "clicked", G_CALLBACK(botaoCancelarClicado), janelaListaAdicionar);

    gtk_widget_show_all(janelaListaAdicionar);
}



void botaoPagamentoClicado(GtkWidget *widget, gpointer data) {
    gtk_stack_set_visible_child_name(GTK_STACK(stackRight), "paginaPagamento");

}

void botaoPagamentoQualquerOKClicado(GtkWidget *widget, gpointer data){

    Salvador = 0;

    const gchar *nomePagamento = g_ptr_array_index(vetorDadosPagamentoQualquer, 0);

    GtkWidget *entryToBePaidPricePagamentoQualquer = GTK_WIDGET(g_ptr_array_index(vetorDadosPagamentoQualquer, 1));
    const gchar *textoToBePaidPricePagamentoQualquer = gtk_entry_get_text(GTK_ENTRY(entryToBePaidPricePagamentoQualquer));

    double PriceToBePaid = strtod(textoToBePaidPricePagamentoQualquer, NULL);

    if(PriceToBePaid > (TotalToBePaidPrice + 0.5)){
        Salvador = 1;

        GtkWidget *janelaErro = gtk_message_dialog_new(NULL, GTK_DIALOG_MODAL, GTK_MESSAGE_ERROR, GTK_BUTTONS_OK, "Valor deve ser menor que o valor total!");

        gtk_dialog_run(GTK_DIALOG(janelaErro));
        gtk_widget_destroy(janelaErro);
        gtk_widget_destroy(janelaPagamentoQualquer);
    }

    if(Salvador == 0){
        TotalToBePaidPrice = TotalToBePaidPrice - PriceToBePaid;

        gtk_label_set_text(GTK_LABEL(labelTextoPagamentoNome[ContadoPagamentoQualquer]), nomePagamento);
        nomesPagamentoQualquerNome[ContadoPagamentoQualquer] = gtk_label_get_text(GTK_LABEL(labelTextoPagamentoNome[ContadoPagamentoQualquer]));


        gchar *arrumadorPricePagamento = g_strdup_printf("R$%.2f", PriceToBePaid);
        gtk_label_set_text(GTK_LABEL(labelTextoPagamentoToBePaidPrice[ContadoPagamentoQualquer]), arrumadorPricePagamento);
        nomesPagamentoQualquerPrice[ContadoPagamentoQualquer] = gtk_label_get_text(GTK_LABEL(labelTextoPagamentoToBePaidPrice[ContadoPagamentoQualquer]));


        arrumadorPricePagamento = g_strdup_printf("R$%.2f", TotalToBePaidPrice);
        gtk_label_set_text(GTK_LABEL(textoPagamentoBotLeftTopPrice), arrumadorPricePagamento);


        gtk_widget_show(labelTextoPagamentoNome[ContadoPagamentoQualquer]);
        gtk_widget_show(labelTextoPagamentoToBePaidPrice[ContadoPagamentoQualquer]);

        ContadoPagamentoQualquer = ContadoPagamentoQualquer + 1;
        g_free(arrumadorPricePagamento);
        gtk_widget_destroy(janelaPagamentoQualquer);
    }
}

void botaoPagamentoQualquerClicado(GtkWidget *widget, gpointer data){
    janelaPagamentoQualquer = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_default_size(GTK_WINDOW(janelaPagamentoQualquer), 200, 200);

    GtkWidget *layoutPagamentoQualquer;
    GtkWidget *layoutPagamentoQualquerTop;
    GtkWidget *layoutPagamentoQualquerMiddle;
    GtkWidget *layoutPagamentoQualquerBot;
    GtkWidget *layoutPagamentoQualquerArrumador;

    GtkWidget *textoEntryToBePaidPrice;

    GtkWidget *botaoPagamentoQualquerOK;
    GtkWidget *botaoPagamentoQualquerCancel;



    layoutPagamentoQualquer = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(janelaPagamentoQualquer), layoutPagamentoQualquer);

    layoutPagamentoQualquerTop = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutPagamentoQualquer), layoutPagamentoQualquerTop);
    gtk_widget_set_hexpand(layoutPagamentoQualquerTop, TRUE);
    gtk_widget_set_vexpand(layoutPagamentoQualquerTop, TRUE);

    layoutPagamentoQualquerMiddle = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutPagamentoQualquer), layoutPagamentoQualquerMiddle);
    gtk_widget_set_hexpand(layoutPagamentoQualquerMiddle, TRUE);
    gtk_widget_set_vexpand(layoutPagamentoQualquerMiddle, TRUE);

    layoutPagamentoQualquerBot = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutPagamentoQualquer), layoutPagamentoQualquerBot);

    layoutPagamentoQualquerArrumador = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutPagamentoQualquer), layoutPagamentoQualquerArrumador);



    textoEntryToBePaidPrice = gtk_label_new("Price to be paid:");
    gtk_container_add(GTK_CONTAINER(layoutPagamentoQualquerTop), textoEntryToBePaidPrice);
    gtk_widget_set_hexpand(textoEntryToBePaidPrice, TRUE);



    textoArrumador = gtk_label_new("");
    gtk_container_add(GTK_CONTAINER(layoutPagamentoQualquerMiddle), textoArrumador);
    gtk_widget_set_hexpand(textoArrumador, TRUE);

    entryToBePaidPrice = gtk_entry_new();
    gtk_container_add(GTK_CONTAINER(layoutPagamentoQualquerMiddle), entryToBePaidPrice);
    gtk_widget_set_hexpand(entryToBePaidPrice, TRUE);
    gtk_widget_set_vexpand(entryToBePaidPrice, TRUE);

    textoArrumador = gtk_label_new("");
    gtk_container_add(GTK_CONTAINER(layoutPagamentoQualquerMiddle), textoArrumador);
    gtk_widget_set_hexpand(textoArrumador, TRUE);


    int valorG = GPOINTER_TO_INT(data);
    vetorDadosPagamentoQualquer = g_ptr_array_new();
    g_ptr_array_add(vetorDadosPagamentoQualquer, (gpointer)nomesPagamentoAdicionarNovoPagamento[valorG]);
    g_ptr_array_add(vetorDadosPagamentoQualquer, entryToBePaidPrice);



    botaoPagamentoQualquerCancel = gtk_button_new_with_label("Cancelar");
    gtk_box_pack_end(GTK_BOX(layoutPagamentoQualquerBot), botaoPagamentoQualquerCancel, FALSE, FALSE, 0);

    botaoPagamentoQualquerOK = gtk_button_new_with_label("OK");
    gtk_box_pack_end(GTK_BOX(layoutPagamentoQualquerBot), botaoPagamentoQualquerOK, FALSE, FALSE, 0);

    g_signal_connect(G_OBJECT(botaoPagamentoQualquerOK), "clicked", G_CALLBACK(botaoPagamentoQualquerOKClicado), vetorDadosPagamentoQualquer);
    g_signal_connect(G_OBJECT(botaoPagamentoQualquerCancel), "clicked", G_CALLBACK(botaoCancelarClicado), janelaPagamentoQualquer);

    gtk_widget_show_all(janelaPagamentoQualquer);
}

void botaoPagamentoAdicionarOKClicado(GtkWidget *widget, gpointer data){

    Salvador = 0;

    GtkWidget *entryNomePagamento = GTK_WIDGET(g_ptr_array_index(vetorDadosPagamento, 0));
    const gchar *textoNomePagamento = gtk_entry_get_text(GTK_ENTRY(entryNomePagamento));

    GtkWidget *entryCodePagamento = GTK_WIDGET(g_ptr_array_index(vetorDadosPagamento, 1));
    const gchar *textoCodePagamento = gtk_entry_get_text(GTK_ENTRY(entryCodePagamento));



    for(g = 0; g < 20; g++){
        if(textoCodePagamento && nomesPagamentoAdicionarNovoCode[g]){
            buscaCode = g_ascii_strcasecmp(textoCodePagamento, nomesPagamentoAdicionarNovoCode[g]);

            if(buscaCode == 0){
                Salvador = 1;
                GtkWidget *janelaErro = gtk_message_dialog_new(NULL, GTK_DIALOG_MODAL, GTK_MESSAGE_ERROR, GTK_BUTTONS_OK, "Code already insert!");

                gtk_dialog_run(GTK_DIALOG(janelaErro));
                gtk_widget_destroy(janelaErro);
                gtk_widget_destroy(janelaPagamentoAdicionar);
            }
        }
    }


    if(Salvador == 0){
        gtk_button_set_label(GTK_BUTTON(botaoPagamentoAdicionarNovo[ContadorPagamento]), textoNomePagamento);
        nomesPagamentoAdicionarNovoPagamento[ContadorPagamento] = g_strdup(textoNomePagamento);
        nomesPagamentoAdicionarNovoCode[ContadorPagamento] = g_strdup(textoCodePagamento);

        gtk_widget_show(botaoPagamentoAdicionarNovo[ContadorPagamento]);

        ContadorPagamento = ContadorPagamento + 1;
        gtk_widget_destroy(janelaPagamentoAdicionar);
    }
}

void botaoPagamentoAdicionarClicado(GtkWidget *widget, gpointer data) {
    janelaPagamentoAdicionar = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_default_size(GTK_WINDOW(janelaPagamentoAdicionar), 400, 200);

    GtkWidget *layoutPagamentoAdicionar;
    GtkWidget *layoutPagamentoAdicionarTop;
    GtkWidget *layoutPagamentoAdicionarMiddle;
    GtkWidget *layoutPagamentoAdicionarBot;
    GtkWidget *layoutPagamentoAdicionarArrumador;

    GtkWidget *textoEntryNomePagamento;
    GtkWidget *textoEntryCode;

    GtkWidget *botaoPagamentoAdicionarOK;
    GtkWidget *botaoPagamentoAdicionarCancel;



    layoutPagamentoAdicionar = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(janelaPagamentoAdicionar), layoutPagamentoAdicionar);

    layoutPagamentoAdicionarTop = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutPagamentoAdicionar), layoutPagamentoAdicionarTop);
    gtk_widget_set_hexpand(layoutPagamentoAdicionarTop, TRUE);
    gtk_widget_set_vexpand(layoutPagamentoAdicionarTop, TRUE);

    layoutPagamentoAdicionarMiddle = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutPagamentoAdicionar), layoutPagamentoAdicionarMiddle);
    gtk_widget_set_hexpand(layoutPagamentoAdicionarMiddle, TRUE);
    gtk_widget_set_vexpand(layoutPagamentoAdicionarMiddle, TRUE);

    layoutPagamentoAdicionarBot = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutPagamentoAdicionar), layoutPagamentoAdicionarBot);

    layoutPagamentoAdicionarArrumador = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutPagamentoAdicionar), layoutPagamentoAdicionarArrumador);



    textoEntryNomePagamento = gtk_label_new("Forma de Pagamento");
    gtk_container_add(GTK_CONTAINER(layoutPagamentoAdicionarTop), textoEntryNomePagamento);
    gtk_widget_set_hexpand(textoEntryNomePagamento, TRUE);

    textoEntryCode = gtk_label_new("Code");
    gtk_container_add(GTK_CONTAINER(layoutPagamentoAdicionarTop), textoEntryCode);
    gtk_widget_set_hexpand(textoEntryCode, TRUE);


    textoArrumador = gtk_label_new("");
    gtk_container_add(GTK_CONTAINER(layoutPagamentoAdicionarMiddle), textoArrumador);
    gtk_widget_set_hexpand(textoArrumador, TRUE);

    entryNomePagamento = gtk_entry_new();
    gtk_container_add(GTK_CONTAINER(layoutPagamentoAdicionarMiddle), entryNomePagamento);
    gtk_widget_set_hexpand(entryNomePagamento, TRUE);
    gtk_widget_set_vexpand(entryNomePagamento, TRUE);

    entryCode = gtk_entry_new();
    gtk_container_add(GTK_CONTAINER(layoutPagamentoAdicionarMiddle), entryCode);
    gtk_widget_set_hexpand(entryCode, TRUE);
    gtk_widget_set_vexpand(entryCode, TRUE);

    textoArrumador = gtk_label_new("");
    gtk_container_add(GTK_CONTAINER(layoutPagamentoAdicionarMiddle), textoArrumador);
    gtk_widget_set_hexpand(textoArrumador, TRUE);


    vetorDadosPagamento = g_ptr_array_new();
    g_ptr_array_add(vetorDadosPagamento, entryNomePagamento);
    g_ptr_array_add(vetorDadosPagamento, entryCode);



    botaoPagamentoAdicionarCancel = gtk_button_new_with_label("Cancelar");
    gtk_box_pack_end(GTK_BOX(layoutPagamentoAdicionarBot), botaoPagamentoAdicionarCancel, FALSE, FALSE, 0);

    botaoPagamentoAdicionarOK = gtk_button_new_with_label("OK");
    gtk_box_pack_end(GTK_BOX(layoutPagamentoAdicionarBot), botaoPagamentoAdicionarOK, FALSE, FALSE, 0);

    g_signal_connect(G_OBJECT(botaoPagamentoAdicionarOK), "clicked", G_CALLBACK(botaoPagamentoAdicionarOKClicado), vetorDadosPagamento);
    g_signal_connect(G_OBJECT(botaoPagamentoAdicionarCancel), "clicked", G_CALLBACK(botaoCancelarClicado), janelaPagamentoAdicionar);

    gtk_widget_show_all(janelaPagamentoAdicionar);
}



void botaoNotinhaClicado(GtkWidget *widget, gpointer data) {
    if(TotalToBePaidPrice > 0.5){
        GtkWidget *janelaErro = gtk_message_dialog_new(NULL, GTK_DIALOG_MODAL, GTK_MESSAGE_ERROR, GTK_BUTTONS_OK, "Pague por todos os produtos!");

        gtk_dialog_run(GTK_DIALOG(janelaErro));
        gtk_widget_destroy(janelaErro);
    } else {

        gint buscaTudo;

        printf("\nL & C VENDAS\n5ª Avenida, 174 - Setor Leste Universitário\n74605-040\n\n\n         NOTA FISCAL\n\nCODE     PRODUTO     QUANTIDADE     PREÇO POR UNIDADE     PREÇO TOTAL\n");

        for(g = 0; g < 100; g++){
            if(nomesListaProduto[g]){
                for(j = 0; j < 100; j++){
                    if(nomesGerenteProduto[j]){

                        buscaTudo = g_ascii_strcasecmp(nomesListaProduto[g], nomesGerenteProduto[j]);

                        if(buscaTudo == 0){
                            printf("%-10s %-10s %-10s %-10s %.2lf\n", nomesGerenteCode[j], nomesListaProduto[g], nomesListaQuantity[g], nomesGerentePrice[j], nomesListaPrice[g]);
                        }
                    }
                }
            }
        }

        printf("------------------------------------------------------------------------------------------------------------------------\n");
        printf("Valor total a pagar: R$%.2lf\n\n", TotalPrice);
        printf("Formas de Pagamento:\n\n");

        for(g = 0; g < 20; g++){
            if(nomesPagamentoQualquerNome[g]){
                for(j = 0; j < 20; j++){
                    if(nomesPagamentoAdicionarNovoPagamento[j]){

                        buscaTudo = g_ascii_strcasecmp(nomesPagamentoQualquerNome[g], nomesPagamentoAdicionarNovoPagamento[j]);

                        if(buscaTudo == 0){
                            printf("%-10s %-10s %-10s\n", nomesPagamentoAdicionarNovoCode[j], nomesPagamentoQualquerNome[g], nomesPagamentoQualquerPrice[g]);
                        }
                    }
                }
            }
        }

        printf("\nObrigado pela preferência!\n");

        gtk_main_quit();
    }
}



void botaoGerenteClicado(GtkWidget *widget, gpointer data) {
    gtk_stack_set_visible_child_name(GTK_STACK(stackRight), "paginaGerente");
}

void botaoGerenteAdicionarOKClicado(GtkWidget *widget, gpointer data){

    Salvador = 0;

    GtkWidget *entryProduto = GTK_WIDGET(g_ptr_array_index(vetorDadosGerente, 0));
    const gchar *textoProduto = gtk_entry_get_text(GTK_ENTRY(entryProduto));

    GtkWidget *entryCode = GTK_WIDGET(g_ptr_array_index(vetorDadosGerente, 1));
    const gchar *textoCode = gtk_entry_get_text(GTK_ENTRY(entryCode));

    GtkWidget *entryPrice = GTK_WIDGET(g_ptr_array_index(vetorDadosGerente, 2));
    const gchar *textoPrice = gtk_entry_get_text(GTK_ENTRY(entryPrice));



    for(g = 0; g < 100; g++){
        if(textoCode && nomesGerenteCode[g]){
            buscaEstoque = g_ascii_strcasecmp(textoCode, nomesGerenteCode[g]);

            if(buscaEstoque == 0){
                Salvador = 1;
                GtkWidget *janelaErro = gtk_message_dialog_new(NULL, GTK_DIALOG_MODAL, GTK_MESSAGE_ERROR, GTK_BUTTONS_OK, "Code already insert!");

                gtk_dialog_run(GTK_DIALOG(janelaErro));
                gtk_widget_destroy(janelaErro);
                gtk_widget_destroy(janelaGerenteAdicionar);
            }
        }
    }



    double Price = strtod(textoPrice, NULL);

    if(Price < 0){
        Salvador = 1;

        GtkWidget *janelaErro = gtk_message_dialog_new(NULL, GTK_DIALOG_MODAL, GTK_MESSAGE_ERROR, GTK_BUTTONS_OK, "Price deve ser maior que R$0,00!");

        gtk_dialog_run(GTK_DIALOG(janelaErro));
        gtk_widget_destroy(janelaErro);
        gtk_widget_destroy(janelaGerenteAdicionar);
    }



    if(Salvador == 0){
        gtk_label_set_text(GTK_LABEL(labelTextoGerenteProduto[ContadorGerente]), textoProduto);
        nomesGerenteProduto[ContadorGerente] = gtk_label_get_text(GTK_LABEL(labelTextoGerenteProduto[ContadorGerente]));

        gtk_label_set_text(GTK_LABEL(labelTextoGerenteCode[ContadorGerente]), textoCode);
        nomesGerenteCode[ContadorGerente] = gtk_label_get_text(GTK_LABEL(labelTextoGerenteCode[ContadorGerente]));

        gtk_label_set_text(GTK_LABEL(labelTextoGerentePrice[ContadorGerente]), textoPrice);
        nomesGerentePrice[ContadorGerente] = gtk_label_get_text(GTK_LABEL(labelTextoGerentePrice[ContadorGerente]));

        ContadorGerente = ContadorGerente + 1;
        gtk_window_close(GTK_WINDOW(janelaGerenteAdicionar));
    }
}

void botaoGerenteAdicionarClicado(GtkWidget *widget, gpointer data){
    janelaGerenteAdicionar = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_default_size(GTK_WINDOW(janelaGerenteAdicionar), 400, 200);

    GtkWidget *layoutGerenteAdicionar;
    GtkWidget *layoutGerenteAdicionarTop;
    GtkWidget *layoutGerenteAdicionarMiddle;
    GtkWidget *layoutGerenteAdicionarBot;
    GtkWidget *layoutGerenteAdicionarArrumador;

    GtkWidget *textoGerenteAdicionarEntryProduto;
    GtkWidget *textoGerenteAdicionarEntryCode;
    GtkWidget *textoGerenteAdicionarEntryPrice;

    GtkWidget *botaoGerenteAdicionarOK;
    GtkWidget *botaoGerenteAdicionarCancel;




    layoutGerenteAdicionar = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(janelaGerenteAdicionar), layoutGerenteAdicionar);

    layoutGerenteAdicionarTop = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutGerenteAdicionar), layoutGerenteAdicionarTop);
    gtk_widget_set_hexpand(layoutGerenteAdicionarTop, TRUE);
    gtk_widget_set_vexpand(layoutGerenteAdicionarTop, TRUE);

    layoutGerenteAdicionarMiddle = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutGerenteAdicionar), layoutGerenteAdicionarMiddle);
    gtk_widget_set_hexpand(layoutGerenteAdicionarMiddle, TRUE);
    gtk_widget_set_vexpand(layoutGerenteAdicionarMiddle, TRUE);

    layoutGerenteAdicionarBot = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutGerenteAdicionar), layoutGerenteAdicionarBot);

    layoutGerenteAdicionarArrumador = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutGerenteAdicionar), layoutGerenteAdicionarArrumador);



    textoGerenteAdicionarEntryProduto = gtk_label_new("Produto");
    gtk_container_add(GTK_CONTAINER(layoutGerenteAdicionarTop), textoGerenteAdicionarEntryProduto);
    gtk_widget_set_hexpand(textoGerenteAdicionarEntryProduto, TRUE);

    textoGerenteAdicionarEntryCode = gtk_label_new("Code");
    gtk_container_add(GTK_CONTAINER(layoutGerenteAdicionarTop), textoGerenteAdicionarEntryCode);
    gtk_widget_set_hexpand(textoGerenteAdicionarEntryCode, TRUE);

    textoGerenteAdicionarEntryPrice = gtk_label_new("Price");
    gtk_container_add(GTK_CONTAINER(layoutGerenteAdicionarTop), textoGerenteAdicionarEntryPrice);
    gtk_widget_set_hexpand(textoGerenteAdicionarEntryPrice, TRUE);



    textoArrumador = gtk_label_new("");
    gtk_container_add(GTK_CONTAINER(layoutGerenteAdicionarMiddle), textoArrumador);
    gtk_widget_set_hexpand(textoArrumador, TRUE);

    entryProduto = gtk_entry_new();
    gtk_container_add(GTK_CONTAINER(layoutGerenteAdicionarMiddle), entryProduto);
    gtk_widget_set_hexpand(entryProduto, TRUE);
    gtk_widget_set_vexpand(entryProduto, TRUE);

    entryCode = gtk_entry_new();
    gtk_container_add(GTK_CONTAINER(layoutGerenteAdicionarMiddle), entryCode);
    gtk_widget_set_hexpand(entryCode, TRUE);
    gtk_widget_set_vexpand(entryCode, TRUE);

    entryPrice = gtk_entry_new();
    gtk_container_add(GTK_CONTAINER(layoutGerenteAdicionarMiddle), entryPrice);
    gtk_widget_set_hexpand(entryPrice, TRUE);
    gtk_widget_set_vexpand(entryPrice, TRUE);

    textoArrumador = gtk_label_new("");
    gtk_container_add(GTK_CONTAINER(layoutGerenteAdicionarMiddle), textoArrumador);
    gtk_widget_set_hexpand(textoArrumador, TRUE);


    vetorDadosGerente = g_ptr_array_new();
    g_ptr_array_add(vetorDadosGerente, entryProduto);
    g_ptr_array_add(vetorDadosGerente, entryCode);
    g_ptr_array_add(vetorDadosGerente, entryPrice);



    botaoGerenteAdicionarCancel = gtk_button_new_with_label("Cancelar");
    gtk_box_pack_end(GTK_BOX(layoutGerenteAdicionarBot), botaoGerenteAdicionarCancel, FALSE, FALSE, 0);

    botaoGerenteAdicionarOK = gtk_button_new_with_label("OK");
    gtk_box_pack_end(GTK_BOX(layoutGerenteAdicionarBot), botaoGerenteAdicionarOK, FALSE, FALSE, 0);

    g_signal_connect(G_OBJECT(botaoGerenteAdicionarOK), "clicked", G_CALLBACK(botaoGerenteAdicionarOKClicado), vetorDadosGerente);
    g_signal_connect(G_OBJECT(botaoGerenteAdicionarCancel), "clicked", G_CALLBACK(botaoCancelarClicado), janelaGerenteAdicionar);

    gtk_widget_show_all(janelaGerenteAdicionar);
}


int main(int argc, char *argv[]) {
    GtkWidget *layoutPrincipal;
    GtkWidget *layoutLeft;
    GtkWidget *layoutRight;

    GtkWidget *botaoLista;
    GtkWidget *botaoPagamento;
    GtkWidget *botaoNotinha;
    GtkWidget *botaoGerente;

    GtkWidget *layoutLista;
    GtkWidget *layoutListaTop;
    GtkWidget *botaoListaAdicionar;
    GtkWidget *layoutListaBot;

    GtkWidget *labelScrolledWindowListaBotProduto;
    GtkWidget *labelScrolledWindowListaBotQuantity;
    GtkWidget *labelScrolledWindowListaBotPrice;


    GtkWidget *layoutPagamento;
    GtkWidget *layoutPagamentoTop;
    GtkWidget *botaoPagamentoAdicionar;
    GtkWidget *layoutPagamentoBot;
    GtkWidget *layoutPagamentoBotLeft;
    GtkWidget *layoutPagamentoBotLeftTop;
    GtkWidget *layoutPagamentoBotLeftBot;

    GtkWidget *scrolledWindowPagamentoBotLeftBot;


    GtkWidget *layoutGerente;
    GtkWidget *layoutGerenteTop;
    GtkWidget *botaoGerenteAdicionar;
    GtkWidget *layoutGerenteBot;

    GtkWidget *labelScrolledWindowGerenteBotProduto;
    GtkWidget *labelScrolledWindowGerenteBotCode;
    GtkWidget *labelScrolledWindowGerenteBotPrice;



    //Formatação de texto:
    GtkWidget *textoControlador;

    GtkWidget *textoNome;
    GtkWidget *textoUFG;

    GtkWidget *textoListaTop;
    GtkWidget *textoPagamentoTop;
    GtkWidget *textoGerenteTop;

    GtkWidget *textoPagamentoBotLeftTop;

    const char *textoArial = "label { font-family: Arial; font-size: 34px; }";
    const char *textoArialPequena = "label { font-family: Arial; font-size: 20px; }";
    const char *textoFantasy = "label { font-family: Fantasy; font-size: 38px; }";
    const char *textoTransparente = "label { color: transparent; }";
    const char *textoSans = "label { font-family: Sans; font-size: 50px; }";

    GError *erro = NULL;
    GtkStyleContext *context;

    GtkCssProvider *providerNome = gtk_css_provider_new();
    GtkCssProvider *providerUFG = gtk_css_provider_new();

    GtkCssProvider *providerTextoControlador = gtk_css_provider_new();
    gtk_css_provider_load_from_data(providerTextoControlador, textoTransparente, -1, &erro);

    GtkCssProvider *providerTextosTop = gtk_css_provider_new();
    gtk_css_provider_load_from_data(providerTextosTop, textoSans, -1, &erro);

    GtkCssProvider *providerTextosArialPequena = gtk_css_provider_new();
    gtk_css_provider_load_from_data(providerTextosArialPequena, textoArialPequena, -1, &erro);


    const char *charNome = "L & C Vendas";
    const char *charUFG = "UFG";
    const char *charArrumador = "\n\n\n\n\n";
    const char *charControlador = "------------------------------------------------------------------------";
    const char *charListaTop = " Lista de Compras ";
    const char *charPagamentoTop = " Pagamento ";
    const char *charGerenteTop = " Estoque ";



    gtk_init(&argc, &argv);

    //*CRIAÇÕES*:
    // Janela Principal:
    principalJanela = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(principalJanela), "Minha Aplicação GTK");

    // GTK_Boxs:
    layoutPrincipal = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(principalJanela), layoutPrincipal);

    layoutLeft = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutPrincipal), layoutLeft);

    layoutRight = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutPrincipal), layoutRight);
    gtk_widget_set_hexpand(layoutRight, TRUE);
    gtk_widget_set_vexpand(layoutRight, TRUE);

    layoutLista = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_widget_set_hexpand(layoutLista, TRUE);
    gtk_widget_set_vexpand(layoutLista, TRUE);

    layoutPagamento = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_widget_set_hexpand(layoutPagamento, TRUE);
    gtk_widget_set_vexpand(layoutPagamento, TRUE);

    layoutGerente = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_widget_set_hexpand(layoutGerente, TRUE);
    gtk_widget_set_vexpand(layoutGerente, TRUE);



    // LAYOUT ESQUERDA TOP:
    textoArrumador = gtk_label_new(charArrumador);
    gtk_container_add(GTK_CONTAINER(layoutLeft), textoArrumador);

    textoNome = gtk_label_new(charNome);
    gtk_css_provider_load_from_data(providerNome, textoArial, -1, &erro);
    context = gtk_widget_get_style_context(textoNome);
    gtk_style_context_add_provider(context, GTK_STYLE_PROVIDER(providerNome), GTK_STYLE_PROVIDER_PRIORITY_APPLICATION);
    g_object_unref(providerNome);
    gtk_container_add(GTK_CONTAINER(layoutLeft), textoNome);

    textoUFG = gtk_label_new(charUFG);
    gtk_css_provider_load_from_data(providerUFG, textoFantasy, -1, &erro);
    context = gtk_widget_get_style_context(textoUFG);
    gtk_style_context_add_provider(context, GTK_STYLE_PROVIDER(providerUFG), GTK_STYLE_PROVIDER_PRIORITY_APPLICATION);
    g_object_unref(providerUFG);
    gtk_container_add(GTK_CONTAINER(layoutLeft), textoUFG);

    textoArrumador = gtk_label_new(charArrumador);
    gtk_container_add(GTK_CONTAINER(layoutLeft), textoArrumador);



    // LAYOUT ESQUERDA BOT:
    botaoLista = gtk_button_new_with_label("Lista de Compras");
    gtk_box_pack_start(GTK_BOX(layoutLeft), botaoLista, TRUE, TRUE, 0);

    botaoPagamento = gtk_button_new_with_label("Pagamento");
    gtk_box_pack_start(GTK_BOX(layoutLeft), botaoPagamento, TRUE, TRUE, 0);

    textoArrumador = gtk_label_new(charArrumador);
    gtk_container_add(GTK_CONTAINER(layoutLeft), textoArrumador);

    botaoNotinha = gtk_button_new_with_label("Gerar Nota Fiscal");
    gtk_box_pack_start(GTK_BOX(layoutLeft), botaoNotinha, TRUE, TRUE, 0);

    textoArrumador = gtk_label_new(charArrumador);
    gtk_container_add(GTK_CONTAINER(layoutLeft), textoArrumador);

    botaoGerente = gtk_button_new_with_label("Gerente");
    gtk_box_pack_start(GTK_BOX(layoutLeft), botaoGerente, TRUE, TRUE, 0);

    textoControlador = gtk_label_new(charControlador);
    context = gtk_widget_get_style_context(textoControlador);
    gtk_style_context_add_provider(context, GTK_STYLE_PROVIDER(providerTextoControlador), GTK_STYLE_PROVIDER_PRIORITY_APPLICATION);
    g_object_unref(providerTextoControlador);
    gtk_container_add(GTK_CONTAINER(layoutLeft), textoControlador);



    // LAYOUT DIREITA TOP:
    stackRight = gtk_stack_new();
    gtk_stack_add_named(GTK_STACK(stackRight), layoutLista, "paginaLista");
    gtk_stack_add_named(GTK_STACK(stackRight), layoutPagamento, "paginaPagamento");
    gtk_stack_add_named(GTK_STACK(stackRight), layoutGerente, "paginaGerente");


    gtk_container_add(GTK_CONTAINER(layoutRight), stackRight);



    // LAYOUT LISTA:
    layoutListaTop = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutLista), layoutListaTop);

    layoutListaBot = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutLista), layoutListaBot);
    gtk_widget_set_hexpand(layoutListaBot, TRUE);
    gtk_widget_set_vexpand(layoutListaBot, TRUE);


    textoListaTop = gtk_label_new(charListaTop);
    context = gtk_widget_get_style_context(textoListaTop);
    gtk_style_context_add_provider(context, GTK_STYLE_PROVIDER(providerTextosTop), GTK_STYLE_PROVIDER_PRIORITY_APPLICATION);
    g_object_unref(providerTextosTop);
    gtk_container_add(GTK_CONTAINER(layoutListaTop), textoListaTop);

    botaoListaAdicionar = gtk_button_new_with_label("Adicionar");
    gtk_box_pack_end(GTK_BOX(layoutListaTop), botaoListaAdicionar, FALSE, FALSE, 0);


    scrolledWindowListaBot = gtk_scrolled_window_new(NULL, NULL);
    gtk_container_set_border_width(GTK_CONTAINER(scrolledWindowListaBot), 10);
    gtk_container_add(GTK_CONTAINER(layoutListaBot), scrolledWindowListaBot);
    gtk_widget_set_hexpand(scrolledWindowListaBot, TRUE);
    gtk_widget_set_vexpand(scrolledWindowListaBot, TRUE);

    layoutScrolledWindowListaBot = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(scrolledWindowListaBot), layoutScrolledWindowListaBot);


    layoutScrolledWindowListaBotProduto = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutScrolledWindowListaBot), layoutScrolledWindowListaBotProduto);
    gtk_widget_set_hexpand(layoutScrolledWindowListaBotProduto, TRUE);

    layoutScrolledWindowListaBotQuantity = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutScrolledWindowListaBot), layoutScrolledWindowListaBotQuantity);
    gtk_widget_set_hexpand(layoutScrolledWindowListaBotQuantity, TRUE);

    layoutScrolledWindowListaBotPrice = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutScrolledWindowListaBot), layoutScrolledWindowListaBotPrice);
    gtk_widget_set_hexpand(layoutScrolledWindowListaBotPrice, TRUE);


    labelScrolledWindowListaBotProduto = gtk_label_new("Produtos");
    context = gtk_widget_get_style_context(labelScrolledWindowListaBotProduto);
    gtk_style_context_add_provider(context, GTK_STYLE_PROVIDER(providerTextosArialPequena), GTK_STYLE_PROVIDER_PRIORITY_APPLICATION);
    g_object_unref(providerTextosArialPequena);
    gtk_container_add(GTK_CONTAINER(layoutScrolledWindowListaBotProduto), labelScrolledWindowListaBotProduto);
    gtk_widget_set_hexpand(labelScrolledWindowListaBotProduto, TRUE);

    labelScrolledWindowListaBotQuantity = gtk_label_new("Quantidade");
    context = gtk_widget_get_style_context(labelScrolledWindowListaBotQuantity);
    gtk_style_context_add_provider(context, GTK_STYLE_PROVIDER(providerTextosArialPequena), GTK_STYLE_PROVIDER_PRIORITY_APPLICATION);
    g_object_unref(providerTextosArialPequena);
    gtk_container_add(GTK_CONTAINER(layoutScrolledWindowListaBotQuantity), labelScrolledWindowListaBotQuantity);
    gtk_widget_set_hexpand(labelScrolledWindowListaBotQuantity, TRUE);

    labelScrolledWindowListaBotPrice = gtk_label_new("Price");
    context = gtk_widget_get_style_context(labelScrolledWindowListaBotPrice);
    gtk_style_context_add_provider(context, GTK_STYLE_PROVIDER(providerTextosArialPequena), GTK_STYLE_PROVIDER_PRIORITY_APPLICATION);
    g_object_unref(providerTextosArialPequena);
    gtk_container_add(GTK_CONTAINER(layoutScrolledWindowListaBotPrice), labelScrolledWindowListaBotPrice);
    gtk_widget_set_hexpand(labelScrolledWindowListaBotPrice, TRUE);



    for(g = 0;  g < 100; g++){
        labelTextoListaProduto[g] = gtk_label_new("");
        gtk_container_add(GTK_CONTAINER(layoutScrolledWindowListaBotProduto), labelTextoListaProduto[g]);
        gtk_widget_set_hexpand(labelTextoListaProduto[g], TRUE);

        labelTextoListaQuantity[g] = gtk_label_new("");
        gtk_container_add(GTK_CONTAINER(layoutScrolledWindowListaBotQuantity), labelTextoListaQuantity[g]);
        gtk_widget_set_hexpand(labelTextoListaQuantity[g], TRUE);

        labelTextoListaPrice[g] = gtk_label_new("");
        gtk_container_add(GTK_CONTAINER(layoutScrolledWindowListaBotPrice), labelTextoListaPrice[g]);
        gtk_widget_set_hexpand(labelTextoListaPrice[g], TRUE);
    }



    //LAYOUT PAGAMENTO:
    layoutPagamentoTop = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutPagamento), layoutPagamentoTop);

    layoutPagamentoBot = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutPagamento), layoutPagamentoBot);
    gtk_widget_set_hexpand(layoutPagamentoBot, TRUE);
    gtk_widget_set_vexpand(layoutPagamentoBot, TRUE);

    layoutPagamentoBotLeft = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutPagamentoBot), layoutPagamentoBotLeft);
    gtk_widget_set_hexpand(layoutPagamentoBotLeft, TRUE);
    gtk_widget_set_vexpand(layoutPagamentoBotLeft, TRUE);

    layoutPagamentoBotRight = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutPagamentoBot), layoutPagamentoBotRight);


    textoPagamentoTop = gtk_label_new(charPagamentoTop);
    context = gtk_widget_get_style_context(textoPagamentoTop);
    gtk_style_context_add_provider(context, GTK_STYLE_PROVIDER(providerTextosTop), GTK_STYLE_PROVIDER_PRIORITY_APPLICATION);
    g_object_unref(providerTextosTop);
    gtk_container_add(GTK_CONTAINER(layoutPagamentoTop), textoPagamentoTop);

    botaoPagamentoAdicionar = gtk_button_new_with_label("Adicionar");
    gtk_box_pack_end(GTK_BOX(layoutPagamentoTop), botaoPagamentoAdicionar, FALSE, FALSE, 0);


    layoutPagamentoBotLeftTop = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutPagamentoBotLeft), layoutPagamentoBotLeftTop);
    gtk_widget_set_hexpand(layoutPagamentoBotLeftTop, TRUE);

    layoutPagamentoBotLeftBot = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutPagamentoBotLeft), layoutPagamentoBotLeftBot);
    gtk_widget_set_hexpand(layoutPagamentoBotLeftBot, TRUE);


    scrolledWindowPagamentoBotLeftBot = gtk_scrolled_window_new(NULL, NULL);
    gtk_container_set_border_width(GTK_CONTAINER(scrolledWindowPagamentoBotLeftBot), 10);
    gtk_container_add(GTK_CONTAINER(layoutPagamentoBotLeftBot), scrolledWindowPagamentoBotLeftBot);
    gtk_widget_set_hexpand(scrolledWindowPagamentoBotLeftBot, TRUE);
    gtk_widget_set_vexpand(scrolledWindowPagamentoBotLeftBot, TRUE);

    layoutScrolledWindowPagamentoBotLeftBot = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(scrolledWindowPagamentoBotLeftBot), layoutScrolledWindowPagamentoBotLeftBot);



    textoPagamentoBotLeftTop = gtk_label_new("   A ser pago:");
    context = gtk_widget_get_style_context(textoPagamentoBotLeftTop);
    gtk_style_context_add_provider(context, GTK_STYLE_PROVIDER(providerTextosArialPequena), GTK_STYLE_PROVIDER_PRIORITY_APPLICATION);
    g_object_unref(providerTextosArialPequena);
    gtk_container_add(GTK_CONTAINER(layoutPagamentoBotLeftTop), textoPagamentoBotLeftTop);

    textoPagamentoBotLeftTopPrice = gtk_label_new("R$0,00");
    context = gtk_widget_get_style_context(textoPagamentoBotLeftTopPrice);
    gtk_style_context_add_provider(context, GTK_STYLE_PROVIDER(providerTextosArialPequena), GTK_STYLE_PROVIDER_PRIORITY_APPLICATION);
    g_object_unref(providerTextosArialPequena);
    gtk_container_add(GTK_CONTAINER(layoutPagamentoBotLeftTop), textoPagamentoBotLeftTopPrice);



    for(g = 0; g < 20; g++){
        botaoPagamentoAdicionarNovo[g] = gtk_button_new_with_label("");
        gtk_box_pack_start(GTK_BOX(layoutPagamentoBotRight), botaoPagamentoAdicionarNovo[g], FALSE, FALSE, 0);

        layoutScrolledWindowPagamentoBotLeftBotCompleto[g] = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
        gtk_container_add(GTK_CONTAINER(layoutScrolledWindowPagamentoBotLeftBot), layoutScrolledWindowPagamentoBotLeftBotCompleto[g]);


        labelTextoPagamentoNome[g] = gtk_label_new("");
        gtk_container_add(GTK_CONTAINER(layoutScrolledWindowPagamentoBotLeftBotCompleto[g]), labelTextoPagamentoNome[g]);
        gtk_widget_set_hexpand(labelTextoPagamentoNome[g], TRUE);

        labelTextoPagamentoToBePaidPrice[g] = gtk_label_new("");
        gtk_container_add(GTK_CONTAINER(layoutScrolledWindowPagamentoBotLeftBotCompleto[g]), labelTextoPagamentoToBePaidPrice[g]);
        gtk_widget_set_hexpand(labelTextoPagamentoToBePaidPrice[g], TRUE);
    }



    //LAYOUT GERENTE:
    layoutGerenteTop = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutGerente), layoutGerenteTop);

    layoutGerenteBot = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutGerente), layoutGerenteBot);
    gtk_widget_set_hexpand(layoutGerenteBot, TRUE);
    gtk_widget_set_vexpand(layoutGerenteBot, TRUE);


    textoGerenteTop = gtk_label_new(charGerenteTop);
    context = gtk_widget_get_style_context(textoGerenteTop);
    gtk_style_context_add_provider(context, GTK_STYLE_PROVIDER(providerTextosTop), GTK_STYLE_PROVIDER_PRIORITY_APPLICATION);
    g_object_unref(providerTextosTop);
    gtk_container_add(GTK_CONTAINER(layoutGerenteTop), textoGerenteTop);

    botaoGerenteAdicionar = gtk_button_new_with_label("Adicionar");
    gtk_box_pack_end(GTK_BOX(layoutGerenteTop), botaoGerenteAdicionar, FALSE, FALSE, 0);


    scrolledWindowGerenteBot = gtk_scrolled_window_new(NULL, NULL);
    gtk_container_set_border_width(GTK_CONTAINER(scrolledWindowGerenteBot), 10);
    gtk_container_add(GTK_CONTAINER(layoutGerenteBot), scrolledWindowGerenteBot);
    gtk_widget_set_hexpand(scrolledWindowGerenteBot, TRUE);
    gtk_widget_set_vexpand(scrolledWindowGerenteBot, TRUE);

    layoutScrolledWindowGerenteBot = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_add(GTK_CONTAINER(scrolledWindowGerenteBot), layoutScrolledWindowGerenteBot);


    layoutScrolledWindowGerenteBotProduto = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutScrolledWindowGerenteBot), layoutScrolledWindowGerenteBotProduto);
    gtk_widget_set_hexpand(layoutScrolledWindowGerenteBotProduto, TRUE);

    layoutScrolledWindowGerenteBotCode = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutScrolledWindowGerenteBot), layoutScrolledWindowGerenteBotCode);
    gtk_widget_set_hexpand(layoutScrolledWindowGerenteBotCode, TRUE);

    layoutScrolledWindowGerenteBotPrice = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(layoutScrolledWindowGerenteBot), layoutScrolledWindowGerenteBotPrice);
    gtk_widget_set_hexpand(layoutScrolledWindowGerenteBotPrice, TRUE);


    labelScrolledWindowGerenteBotProduto = gtk_label_new("Produtos");
    context = gtk_widget_get_style_context(labelScrolledWindowGerenteBotProduto);
    gtk_style_context_add_provider(context, GTK_STYLE_PROVIDER(providerTextosArialPequena), GTK_STYLE_PROVIDER_PRIORITY_APPLICATION);
    g_object_unref(providerTextosArialPequena);
    gtk_container_add(GTK_CONTAINER(layoutScrolledWindowGerenteBotProduto), labelScrolledWindowGerenteBotProduto);
    gtk_widget_set_hexpand(labelScrolledWindowGerenteBotProduto, TRUE);

    labelScrolledWindowGerenteBotCode = gtk_label_new("Code");
    context = gtk_widget_get_style_context(labelScrolledWindowGerenteBotCode);
    gtk_style_context_add_provider(context, GTK_STYLE_PROVIDER(providerTextosArialPequena), GTK_STYLE_PROVIDER_PRIORITY_APPLICATION);
    g_object_unref(providerTextosArialPequena);
    gtk_container_add(GTK_CONTAINER(layoutScrolledWindowGerenteBotCode), labelScrolledWindowGerenteBotCode);
    gtk_widget_set_hexpand(labelScrolledWindowGerenteBotCode, TRUE);

    labelScrolledWindowGerenteBotPrice = gtk_label_new("Price");
    context = gtk_widget_get_style_context(labelScrolledWindowGerenteBotPrice);
    gtk_style_context_add_provider(context, GTK_STYLE_PROVIDER(providerTextosArialPequena), GTK_STYLE_PROVIDER_PRIORITY_APPLICATION);
    g_object_unref(providerTextosArialPequena);
    gtk_container_add(GTK_CONTAINER(layoutScrolledWindowGerenteBotPrice), labelScrolledWindowGerenteBotPrice);
    gtk_widget_set_hexpand(labelScrolledWindowGerenteBotPrice, TRUE);



    for(g = 0;  g < 100; g++){
        labelTextoGerenteProduto[g] = gtk_label_new("");
        gtk_container_add(GTK_CONTAINER(layoutScrolledWindowGerenteBotProduto), labelTextoGerenteProduto[g]);
        gtk_widget_set_hexpand(labelTextoGerenteProduto[g], TRUE);

        labelTextoGerenteCode[g] = gtk_label_new("");
        gtk_container_add(GTK_CONTAINER(layoutScrolledWindowGerenteBotCode), labelTextoGerenteCode[g]);
        gtk_widget_set_hexpand(labelTextoGerenteCode[g], TRUE);

        labelTextoGerentePrice[g] = gtk_label_new("");
        gtk_container_add(GTK_CONTAINER(layoutScrolledWindowGerenteBotPrice), labelTextoGerentePrice[g]);
        gtk_widget_set_hexpand(labelTextoGerentePrice[g], TRUE);
    }



    // BOTÕES:
    g_signal_connect(G_OBJECT(botaoLista), "clicked", G_CALLBACK(botaoListaClicado), NULL);
    g_signal_connect(G_OBJECT(botaoPagamento), "clicked", G_CALLBACK(botaoPagamentoClicado), NULL);
    g_signal_connect(G_OBJECT(botaoNotinha), "clicked", G_CALLBACK(botaoNotinhaClicado), NULL);
    g_signal_connect(G_OBJECT(botaoGerente), "clicked", G_CALLBACK(botaoGerenteClicado), NULL);

    g_signal_connect(G_OBJECT(botaoListaAdicionar), "clicked", G_CALLBACK(botaoListaAdicionarClicado), NULL);
    g_signal_connect(G_OBJECT(botaoPagamentoAdicionar), "clicked", G_CALLBACK(botaoPagamentoAdicionarClicado), NULL);
    g_signal_connect(G_OBJECT(botaoGerenteAdicionar), "clicked", G_CALLBACK(botaoGerenteAdicionarClicado), NULL);

    for(g = 0; g < 20; g++){
        g_signal_connect(G_OBJECT(botaoPagamentoAdicionarNovo[g]), "clicked", G_CALLBACK(botaoPagamentoQualquerClicado), GINT_TO_POINTER(g));
    }



    gtk_widget_show_all(principalJanela);



    for(g = 0; g < 20; g++){
        gtk_widget_hide(botaoPagamentoAdicionarNovo[g]);
        gtk_widget_hide(labelTextoPagamentoNome[g]);
        gtk_widget_hide(labelTextoPagamentoToBePaidPrice[g]);
    }

    gtk_main();

    return 0;
}
