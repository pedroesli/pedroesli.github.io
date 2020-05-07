---
layout: post
title: Experiência na criação de um jogo
subtitle: Criando um jogo durante a quarentena
tags: [game-dev]
---

Bom a quarentena tinha começado e junto veio o tedio. Ai pensei, porque não fazer um jogo no meu game engine favorito ,Unity. E bom aqui ficou o resultado do jogo caso voce queira dar uma olhada [Space Platform][1bc24e0c]. Mas em fim, não estou aqui para falar sobre o resultado final do jogo e sim como foi a minha experiência na criação dele.

E o que achei mais interessante foi na hora de programação foi que quanto mais complexo ficava o jogo, mais bagunçado ficava o código, igual ao meu quarto durante as ferias.
Por exemplo, quando o personagem se machucar por algum meio no jogo ele deve subtrair uma vida do total que ele tem. Implementando esta logica no jogo, cada vez que o jogador se machucava ele deveria informar ao componente que é responsável de mostrar a vida e também a todos os objetos que precisavam saber. Como o Game Manager( Gerente do jogo), sistema de respawn , UI Manager e entre outros componentes.

```
public class Player : MonoBehaviour
{
    int vida = 100;

    public void Dano(int dano){
      vida -= dano;
      UIManager.instance.BaraVida(vida);
      if(health < 0)
        GameManager.instance.GameOver();
      else
        Respawn();
    }
}
```
![old-pattern](/img/jogador-old-pattern.png)

Ai decidi fazer uma pequena pesquisa e descobri um padrão de programação chamado de Observer pattern onde basicamente o que ele faz é notificar a todos que estão escutando a um evento.
Basicamente o que ele fala é: "Oh aconteceu este evento! Quem estiver escutando, notificar ele!".

![observer-patter](/img/jogador-observer-patter.png)

```
public class Player : MonoBehaviour
{
    public static Action<int> OnPlayerMachucado;
    int vida = 100;

    public void Dano(int dano){
      if(OnPlayerMachucado!=null)
        OnPlayerMachucado(vida);
    }
}
```
Aqui criei uma ação chamado de OnPlayerMachucado onde ele obri

```
public class UIManager : MonoBehaviour
{

    public void OnEnable(){
        Player.OnPlayerMachucado += Machucado;
    }

    public void OnDisable(){
      Player.OnPlayerMachucado -= Machucado;
    }

    public void Machucado(int vida){
      //Atualizar algum UI
      Debug.log("Vida: " + vida);
    }
}
```
```
public class GameManager : MonoBehaviour
{

    public void OnEnable(){
        Player.OnPlayerMachucado += Machucado;
    }

    public void OnDisable(){
      Player.OnPlayerMachucado -= Machucado;
    }

    public void Machucado(int vida){
      if(vida < 0)
        GameOver();
    }

    public void GameOver(){
      Debug.log("Game Over!");
    }
}
```



  [1bc24e0c]: https://pedroesli.itch.io/space-platform "My space platform game"
