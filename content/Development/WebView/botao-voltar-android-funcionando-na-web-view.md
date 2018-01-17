Isso é necessário para que as teclas, físicas ou nao, que tem como função Voltar, funcionem na WebView, e não feche a sua aplicação, ou volte para a aplicação anterior.

1. Acesse sua Main.Activity 
2. Adicione a seguinte função
```
    @Override
    public void onBackPressed() {
        WebView mWebView = (WebView)findViewById(R.id.webView);
        if (mWebView.canGoBack()) {
            mWebView.goBack();
        } else {
            super.onBackPressed();
        }
    }
```

#### Pronto, agora é só rodar só seu APP

###### Matheus Morett