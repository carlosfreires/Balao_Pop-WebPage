# 🎈 Balão Pop — Jogo Simples em HTML, CSS e JavaScript

[![Jogue agora](https://img.shields.io/badge/Jogue_Agora-🚀-brightgreen)](https://carlosfreires.github.io/Balao_Pop-WebPage/)

Este repositório contém um mini-jogo em que balões sobem pela tela e o jogador deve estourá-los clicando neles.
Cada balão estourado aumenta a pontuação exibida no canto superior esquerdo.

O projeto é totalmente feito em HTML + CSS + JavaScript puro, sem bibliotecas externas, em um único arquivo (index.html) e com menos de 50 linhas de código.

## 📌 O Jogo

Balões coloridos aparecem continuamente na tela e sobem até desaparecer.
Se o usuário clicar em um balão, ele estoura, soma pontos e toca um pequeno som "pop".

## 📁 Estrutura do Projeto

```bash
/
├── index.html       # Arquivo contendo todo o código (HTML, CSS e JS)
└── README.md        # Documentação do projeto
```

## Tecnologias usadas

* **HTML5**

* **CSS3** (animações, gradient)  

* **JavaScript** (DOM, Web Audio API)

## 🧠 Código linha a linha

Abaixo segue a explicação completa do funcionamento do código.

### 📄 HTML - Estrutura Base

```html
<!DOCTYPE html>
<html lang="pt-BR">

<head>
    <title>Balão Pop</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
       /* ... */
    </style>
</head>
<body>
    <div id="s">0</div>

    <script>
        /* ... */
    </script>
</body>
</html>
```

* ```<!DOCTYPE html>```

    Define que o documento usa a versão HTML5.

* ```<html lang="pt-BR">```

    Abre o documento HTML e define o idioma como português do Brasil.

* ```<head>```
    Define a seção onde ficam metadados do documento.

* ```<title>Balão Pop</title>```

    Define o título da aba do navegador.

* ```<meta charset="UTF-8">```

    Especifica que o documento usa codificação UTF-8 (suporta acentos).

* ```<meta name="viewport" content="width=device-width, initial-scale=1.0">```

    Garante que a página seja responsiva em dispositivos móveis.

* ```<style>```

    Abre a seção de estilos CSS embutidos. (CSS na próxima seção).

* ```<body>```

    Área visível da página.

* ```<div id="s">0</div>```

    Cria um elemento fixo na tela para exibir a pontuação inicial 0.

    ```id="s"``` → usado no JavaScript para atualizar o placar.

* ```<script>```

    código JavaScript. (Explicado na seção "JavaScript").

### 🎨 CSS - Estilos e Animações

#### 🔹 Estilo geral

```css
body {
    margin: 0;
    overflow: hidden;
    background: linear-gradient(135deg, #ff9aff, #7afcff, #b28dff);
    font-family: monospace;
}
```

* Remove margens do body.

* ```overflow: hidden``` impede barras de rolagem, essencial para o jogo.

* Define um **degradê colorido suave** como fundo.

* Usa a fonte *monospace*.

#### **🔹 Pontuação na tela**

```css
#s {
    position: fixed;
    top: 10px;
    left: 10px;
    padding: 6px 12px;
    background: #fff3;
    border-radius: 8px;
    color: #000;
    font-size: 18px;
}
```

* Este elemento ```#s``` exibe a pontuação.

* Fixado no canto superior esquerdo.

* Fundo semitransparente.

* Fonte maior e bordas arredondadas.

#### **🔹 Estilo dos balões**

```css
.b {
    position: absolute;
    width: 50px;
    height: 65px;
    border-radius: 50% 50% 45% 45%;
    cursor: pointer;
    animation: up 6s linear;
    display: flex;
    justify-content: center;
}
```

* ```.b``` representa cada balão (Cada balão é uma ```<div>```).

* São elementos posicionados de forma absoluta.

* O formato oval é obtido com border-radius: 50%.

* O cursor vira "mãozinha".

* Cada balão usa a animação ```up``` com duração de 6 segundos.

#### **🔹 Cordinha do balão**

```css
.b::after {
    content: "";
    position: absolute;
    bottom: -40px;
    width: 2px;
    height: 40px;
    background: #0004;
    border-radius: 2px;
}
```

* Pseudoelemento que cria a "cordinha" do balão.

#### **🔹 🎬 Animação dos balões**

```css
@keyframes up {
    from { transform: translateY(100vh); }
    to   { transform: translateY(-150px); }
}
```

* Faz o balão sair da parte inferior da tela (100vh) e subir além do topo (-150px).

### ⚙️ JavaScript — Lógica do Jogo

#### **🔹 Variáveis iniciais**

```js
let c = 0, a = new AudioContext();
```

* ```c``` armazena a pontuação (contador da pontuação).

* ```a``` cria um contexto de áudio usado para gerar o som do “pop”.

#### **🔹 🔊 Função do som de estouro (Pop)**

```js
function popSound() {
    let o = a.createOscillator(), g = a.createGain();
    o.connect(g);
    g.connect(a.destination);
    
    o.frequency.value = 600;
    g.gain.setValueAtTime(.3, a.currentTime);
    
    o.start();
    o.stop(a.currentTime + .1);
}
```

**Passo a passo:**

1. Cria um oscilador ```o``` (gerador de onda sonora (tom)).

2. Cria um ganho ```g``` (controle de volume (Define volume)).

3. Conecta o oscilador ao ganho e o ganho à saída de áudio.

4. Define frequência em 600 Hz (som curto e agudo).

5. Define volume inicial como ```0.3```.

6. Começa a tocar imediatamente.

7. Para após 0.1 segundos, criando o efeito "pop".

#### **🔹 🎈 Função que cria um novo balão**

```js
function newB(){
        let b = document.createElement("div");
        b.className = "b";
        b.style.left = Math.random() * (innerWidth-60) + "px";
        b.style.background = `hsl(${Math.random()*360} 80% 70% / .85)`;
        b.onclick = () => {
            c++; s.textContent = c; popSound(); b.remove();
        };
        document.body.appendChild(b);
        setTimeout( () => b.remove(), 6500);
    }
```

1. Cria um novo elemento ```<div>``` representando o balão.

2. Atribui a classe ```.b```.

3. Define posição horizontal aleatória.

4. Garante que o balão fique dentro da tela.

5. Define uma cor aleatória usando o espaço de cor HSL.

6. Saturação de 80%, luminosidade de 70%, opacidade de 0.85.

7. **🖱️ Evento de clique no balão**

    * Incrementa a pontuação.

    * Atualiza o texto de ```#s``` (atualiza o placar).

    * Reproduz o som.

    * Remove o balão da tela.

8. Adiciona o balão ao corpo do documento.

9. Remove automaticamente após 6.5s (atraso um pouco maior que a animação, para garantir limpeza).

10. **⏱️ Criação contínua de balões**

    * A cada 650ms, um novo balão é criado.

## ▶️ Como Executar o Projeto Localmente

1. Clonar o repositório

```bash
git clone https://github.com/carlosfreires/Balao_Pop-WebPage.git
```

2. Acessar a pasta

```bash
cd Balao_Pop-WebPage
```

3. Rodar o jogo

Como é apenas um arquivo .html, você pode abrir diretamente no navegador:

* clique duas vezes no arquivo index.html

ou

* abra pelo terminal:

No windows:

```bash
start index.html
```

No macOS

```bash
open index.html
```

No Linux:

```bash
xdg-open index.html
```

Não é necessário servidor local.

## 🧪 Requisitos

* Qualquer navegador moderno (Chrome, Firefox, Edge, Safari).

* Suporte a Web Audio API (presente em todos os navegadores atuais).

## 🤝 Contribuições

Sinta-se livre para enviar melhorias, animações extras ou novas mecânicas!
Issues e pull requests são bem-vindos.

## 📜 Licença

Este projeto pode ser utilizado livremente.
