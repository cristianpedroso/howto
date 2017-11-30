# Setup Up Web View - IOS

#### Criando a web view na main activity

1. Acesse o arquivo Main.storyboard.
![](https://i.imgur.com/aCzowPZ.png)

2. No canto superior direito, com o 3 icone do menu de cima selecionado, busque bot WebKit View
![](https://imgur.com/slNPemm.png)

3. Arraste para o device, e redimensione para que ocupe 100% da tela.
![](https://imgur.com/yfQK2am.png)

4. Ainda no canto superior direito, abra essa opcao do menu, e selecione os 4 cantos da tela, deixando os palitinhos vermelhos, após isso, clique em Add 4 Constraints. Isso irá garantir que sua WebView nao fique com margens no aplicativo.
![](https://imgur.com/nLtjjko.png)

4. Entre na sua View Controller
![](https://imgur.com/nUybF2j.png)
5. Certifique-se de importar o UIKit e o WebKit
  ```
     import UIKit
     import WebKit
```

6. Sobreescreva o metodo ViewDidLoad
```
    @IBOutlet weak var myWebView: WKWebView!
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Url da sua aplicacao
        let myURL = URL(string: "http://letsrock.hero99.com.br/clientes/adrenalyze-app/")
        let myRequest = URLRequest(url: myURL!)
        
        // Configuracoes da WebView. (Podem variar de acordo com o projeto)
        myWebView.uiDelegate = self
        myWebView.navigationDelegate = self
        myWebView.scrollView.bounces = false
        myWebView.scrollView.isScrollEnabled = false
        myWebView.scrollView.pinchGestureRecognizer?.isEnabled = false
        myWebView.load(myRequest)
        
    }
```

#### Pronto, agora é só rodar só seu APP
