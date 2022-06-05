---
layout: post
title: Ciclo de vida do watchOS no SwiftUI
subtitle: Como responder aos eventos de ciclo de vida do watchOS?
cover-img: /img/cover-purple.png
black-header: true
comments: true
css: "/css/post.css"
tags: [watchOS]
---

Você está precisando entender como funciona e como responder aos eventos do ciclo de vida do seu aplicativo? Bom, então já está num bom caminho. Estarei mostrando duas formas no qual você vai poder ter esse controle. A escolha de qual implementar vai depender da aquele que melhor resolveria o problema da sua aplicação. Más antes de mostrar essas duas formas, acho importante entender como funciona o ciclo de vida do watchOS, no qual veremos em seguida.

# Ciclo de vida do watchOS

| Estado | State | Descrição |
| --- | --- | --- |
| Não esta em execução | Not running | O aplicativo watchOS não está em execução. O usuário não iniciou o aplicativo watchOS ou o sistema suspendeu e depois limpou o aplicativo. |
| Inativo | Inactive | O aplicativo watchOS está sendo executado em primeiro plano, mas não está recebendo ações de controles ou gestos; no entanto, ele pode estar executando outro código. Um aplicativo recém-lançado geralmente permanece nesse estado por um brevemente momento enquanto faz a transição para o estado ativo. Um aplicativo ativo que faz a transição para esse estado deve se silenciar e se preparar para a transição ao segundo plano. |
| Ativo | Active | O aplicativo watchOS está sendo executado em primeiro plano e recebendo ações de controles e gestos. Este é o modo normal para aplicativos em execução na tela. |
| Segundo Plano | Background | O sistema deu ao aplicativo watchOS uma pequena quantidade de tempo de execução em segundo plano. O sistema fornece tempo de execução em segundo plano aos aplicativos ao executar uma sessão em segundo plano, executar tarefas em segundo plano e antes de suspender o aplicativo. |
| Suspendido | Suspended | O aplicativo está na memória, mas não está executando o código. O sistema suspende aplicativos que estão em segundo plano e não têm tarefas pendentes para serem concluídas. |

# 1ª forma usando WKExtensionDelegate do WatchKit

`WKExtensionDelegate` Serve para responder aos eventos do ciclo de vida do seu aplicativo, como a ativação e desativação do seu aplicativo. Você também pode implementar métodos delegados para responder a tarefas em segundo plano, intenções de Siri, sessões de treino ou atividade de Handoff de outros dispositivos. Mas para nós, estamos mais interessados nos métodos de ciclo de vida.

## Como implementar

Para utilizar `WKExtensionDelegate` corretamente primeiro precisamos criar uma classe nova que conforma com `NSObject`, `WKExtensionDelegate` e `ObservableObject`

```swift
import WatchKit

class ExtensionDelegate: NSObject, WKExtensionDelegate, ObservableObject {

}
```

E depois implementamos os métodos que vão responder aos eventos de ciclo de vida do nosso aplicativo.

```swift
import WatchKit

class ExtensionDelegate: NSObject, WKExtensionDelegate, ObservableObject {

    func applicationDidFinishLaunching() {
        print("Application did finish launching 🤟")
    }

    func applicationWillEnterForeground() {
        print("Application will enter foreground 😉")
    }

    func applicationDidBecomeActive() {
        print("Application did become active 🥳")
    }

    func applicationWillResignActive() {
        print("Application will resign active (About to deactive app)😉")
    }

		func applicationDidEnterBackground() {
        print("Application did enter Background 👻")
    }

}
```

Em seguida, use o wrapper de propriedade `WKExtensionDelegateAdaptor` dentro da declaração do aplicativo para informar ao SwiftUI sobre o tipo de delegado

```swift
@main
struct MyApp: App {

    @WKExtensionDelegateAdaptor private var extensionDelegate: ExtensionDelegate

    @SceneBuilder var body: some Scene {
        WindowGroup {
            NavigationView {
                ContentView()
            }
        }
    }

}
```

Se o seu delegado de extensão estiver em conformidade com o protocolo `ObservableObject`, como no exemplo acima, o SwiftUI colocará o delegado que ele cria no `Environment`. Você pode acessar o delegado de qualquer cena ou visualização em sua extensão usando o wrapper de propriedade `EnvironmentObject`:

```swift
@EnvironmentObject private var extensionDelegate: MyExtensionDelegate
```

Isso permite que você use o prefixo de dólar 💵 ($) para obter uma associação às propriedades `@Published` que você declara no delegado.

Mas em que ordem são chamados esses métodos?

![https://docs-assets.developer.apple.com/published/74077a8107/ec07a686-2315-4700-9415-6485cc3bcfff.png](https://docs-assets.developer.apple.com/published/74077a8107/ec07a686-2315-4700-9415-6485cc3bcfff.png)

Na figura:

- **Transição A:**  Quando o app muda de ***não está em execução*** para o estado ***inativo*** ou ***segundo plano***, ******o sistema vai chamar o método do delegate `applicationDidFinishLaunching()` .
- **Transição B:** Quando o app faz a transição para o estado ***ativo***, o sistema chama o método `applicationDidBecomeActive()` e quando faz a transição para o ***inativo*** ele chama o `applicationWillResignActive()`
- **Transição C:**  Ao fazer a transição entre os estados em ***segundo plano*** e ***inativo***, o sistema chama o método `applicationWillEnterForeground()` ou `applicationDidEnterBackground()`.

Essa explicação foi dada pela a própria documentação da apple, más pra mim foi um pouco confuso entender de primeira. Então para explicar melhor, vamos imaginar numa situação onde o app não está em execução, e o usuário decide abrir ele e logo em seguida fecha ele. o métodos seriam chamados na seguinte ordem respetivamente:

**Abrindo o app:**

1. `applicationDidFinishLaunching()`
2. `applicationWillEnterForeground()`
3. `applicationDidBecomeActive()`

**Fechando o app:**

1. `applicationWillResignActive()`
2. `applicationDidEnterBackground()`

## 2ª usando ScenePhase do SwiftUI

Outra forma de determinar o estado do ciclo de vida é usando o valor `scenePhase` no `Environment` no qual é possível observar se a sena está ***ativa***, ***inativa*** ou em ***segundo plano.***

```swift
@Environment(\.scenePhase) private var scenePhase
```

Para observar a mudança do estado é simplesmente utilizar o método `onChange(of:perform:)` como mostra no exemplo em seguida:

```swift
@main
struct MyApp: App {
    @Environment(\.scenePhase) private var scenePhase

    var body: some Scene {
        WindowGroup {
            MyRootView()
        }
        .onChange(of: scenePhase) { phase in
            if phase == .background {
              // Executar limpeza quando todas as cenas dentro do
              // MyApp forem para segundo plano.
            }
        }
    }
}
```

## Bibliografia

[https://developer.apple.com/documentation/watchkit/wkextensiondelegate/working_with_the_watchos_app_life_cycle](https://developer.apple.com/documentation/watchkit/wkextensiondelegate/working_with_the_watchos_app_life_cycle)

[https://developer.apple.com/documentation/watchkit/wkextensiondelegate](https://developer.apple.com/documentation/watchkit/wkextensiondelegate)

[https://developer.apple.com/documentation/swiftui/wkextensiondelegateadaptor](https://developer.apple.com/documentation/swiftui/wkextensiondelegateadaptor)

[https://developer.apple.com/documentation/swiftui/scenephase](https://developer.apple.com/documentation/swiftui/scenephase)
