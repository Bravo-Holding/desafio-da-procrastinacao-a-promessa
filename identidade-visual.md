# Identidade Visual — "Cartaz de Guerra"

Sistema de cor extraído de cartaz de campanha em serigrafia sobre parede suja.
Eixo emocional: **medo, peso, urgência**. Não é elegante. É desconfortável de propósito.

Extração feita por amostragem de pixel da referência (228x300 px), não estimada a olho:
o dark é a média dos pixels com valor < 10%, o vermelho é a média dos pixels saturados
em matiz 340-20 graus, o creme é a média dos pixels claros com saturação < 22%.

---

## 1. As três cores-raiz

| Token | Hex | RGB | HSL | Nome |
|---|---|---|---|---|
| `--void` | `#0A0907` | 10, 9, 7 | 40, 18%, 3% | Preto de tinta, quente (não é preto puro) |
| `--blood` | `#A61312` | 166, 19, 18 | 0, 80%, 36% | Vermelho oxidado, cor de sangue seco |
| `--bone` | `#D7C4B4` | 215, 196, 180 | 27, 30%, 77% | Creme papel envelhecido, osso |

**Por que funciona:** o preto é quente (matiz 40), não neutro. Isso faz o vermelho parecer
sujo em vez de vibrante, e o creme parecer papel velho em vez de bege de decoração.
Se você trocar o `#0A0907` por `#000000`, o sistema inteiro perde o peso e vira "dark mode genérico".

---

## 2. Paleta completa

### Escuros (fundos, superfícies)
| Token | Hex | Uso |
|---|---|---|
| `--void` | `#0A0907` | Fundo base da página. O default absoluto. |
| `--pitch` | `#131110` | Superfície elevada leve (cards, blocos internos). |
| `--ash` | `#1C1815` | Card sobre card, inputs, áreas de destaque frio. |
| `--iron` | `#2A2421` | Bordas, divisores, hairlines. Nunca como fundo grande. |

### Vermelhos (acento, alarme)
| Token | Hex | Uso |
|---|---|---|
| `--blood` | `#A61312` | Acento principal. **Só como preenchimento de bloco**, com texto claro em cima. |
| `--blood-deep` | `#6E0F0E` | Sombra/ledge 3D de botão, estado pressionado, fundo de faixa. |
| `--blood-hot` | `#C4181A` | Hover de botão. |
| `--signal` | `#E0392A` | Única variante permitida como texto vermelho sobre fundo escuro, **e só em tamanho display** (ver §3). |
| `--scab` | `#2A0A09` | Fundo de alerta/erro/bloco de dor. Vermelho quase preto. |

### Cremes (texto, papel)
| Token | Hex | Uso |
|---|---|---|
| `--bone-hi` | `#EFE4D8` | Títulos, números grandes, alta ênfase. |
| `--bone` | `#D7C4B4` | Texto corrido. Cor padrão do body. |
| `--bone-mute` | `#9C8D82` | Texto secundário, legendas, labels. |
| `--bone-low` | `#857870` | Terceiro nível de texto (metadado, nota de rodapé de bloco). Limite do AA. |
| `--bone-dim` | `#6B6058` | Texto desativado, placeholder. Nunca para informação necessária. |

**A rampa de escuros só sustenta DOIS níveis de texto**, não três. Os ratios acima valem
sobre `--void`; como acontece com `--signal` (§3), eles caem conforme a superfície sobe:

| token | void | pitch | ash | iron |
|---|---|---|---|---|
| `--bone` | 11,8 | 11,2 | 10,4 | 9,1 |
| `--bone-mute` | 6,2 | 5,9 | 5,5 | **4,8** |
| `--bone-low` | 4,7 | 4,4 | **4,1** | **3,6** |

`--bone` e `--bone-mute` passam AA em toda a rampa. `--bone-low` **só passa sobre `--void`**
— use apenas onde o fundo é comprovadamente `--void` (rodapé, por exemplo). Qualquer token
global de texto aponta para `--bone-mute`, nunca para `--bone-low`, porque token global pode
cair em qualquer superfície.

Não tente inventar um terceiro nível clareando o `--bone-low`: o valor que passaria sobre
`--iron` é `#9A8B80`, a um décimo do `--bone-mute`. O degrau não existe. Aceite dois.

`--bone-hi` fica reservado a títulos e números grandes. `--bone-dim` (3,2:1) é decorativo.

---

## 3. Contraste (WCAG, medido)

Sobre `--void #0A0907`:

| Cor | Ratio | Veredito |
|---|---|---|
| `--bone-hi` `#EFE4D8` | 15,9:1 | AAA |
| `--bone` `#D7C4B4` | 11,8:1 | AAA |
| `--bone-mute` `#9C8D82` | 6,2:1 | AA em qualquer tamanho |
| `--bone-low` `#857870` | 4,7:1 | AA (limite) |
| `--bone-dim` `#6B6058` | 3,2:1 | Só decorativo ou texto grande |
| `--signal` `#E0392A` | 4,5:1 | AA (limite exato) |
| `--blood` `#A61312` | **2,6:1** | **REPROVA. Nunca como texto sobre fundo escuro.** |

Sobre `--blood #A61312`:

| Cor | Ratio | Veredito |
|---|---|---|
| `#FFFFFF` | 7,7:1 | AAA |
| `--bone-hi` `#EFE4D8` | 6,2:1 | AA |
| `--bone` `#D7C4B4` | 4,6:1 | AA só em texto grande (18px+ bold / 24px+) |
| `--void` `#0A0907` | 2,6:1 | Reprova |

### `--signal` na rampa de escuros

`--signal` não tem um contraste, tem quatro. Ele cai conforme a superfície sobe:

| fundo | ratio |
|---|---|
| `--void` `#0A0907` | 4,54:1 |
| `--pitch` `#131110` | 4,29:1 |
| `--ash` `#1C1815` | 4,02:1 |
| `--iron` `#2A2421` | 3,49:1 |

Ou seja: `--signal` só alcança o limiar de 4,5:1 de **texto pequeno** sobre `--void`, e em
mais nenhuma superfície. Mas alcança o limiar de 3:1 de **texto grande** em todas elas.

**Daí a regra de tamanho: `--signal` é cor de display, nunca de texto pequeno.** Use só a
partir de 24px em peso normal ou 18,7px em bold. Abaixo disso, nenhum vermelho: o texto vai
para `--bone-hi` e o vermelho migra para onde ele funciona, que é o bloco — uma borda, um
carimbo, um preenchimento.

A tentação aqui é clarear o vermelho até ele passar em tudo. Não faça: o valor que passa
4,5:1 sobre `--iron` é `#F05A3E`, que já é coral e destrói o sangue seco que define o
sistema. O tamanho é a variável certa a mexer, não a cor.

**Regra prática:** vermelho é bloco, não é letra. Palavra vermelha sobre escuro só em
tamanho display, com `--signal`. Palavra dentro de bloco vermelho, `#FFFFFF` ou `--bone-hi`.
Texto pequeno, nunca vermelho.

---

## 4. Proporção de uso (a regra que sustenta o clima)

```
70%  --void e derivados escuros
22%  --bone / --bone-hi (texto e áreas de respiro)
 8%  vermelho (--blood e família)
```

O vermelho é escasso por desenho. Ele só aparece onde há **dor, urgência ou ação**.
Se mais de um vermelho competir na mesma dobra da tela, o alarme vira ruído e a cor
deixa de significar qualquer coisa. Um vermelho por dobra.

---

## 5. Componentes

**Botão primário**
```
fundo        --blood #A61312
texto        #FFFFFF
ledge 3D     0 4px 0 --blood-deep #6E0F0E
hover        fundo --blood-hot #C4181A
pressionado  translateY(2px), ledge 2px
```

**Botão de checkout — exceção deliberada**
```
fundo        #16A34A
texto        #FFFFFF
ledge 3D     0 4px 0 #0E6B34
hover        #15803D
```
Verde é a única cor fora do sistema de três. Fica por decisão de performance: é a
variável de conversão já validada em produção e não se troca cor de CTA junto com uma
mudança de identidade, senão não se sabe o que causou o efeito. Vale só para o botão
de compra. Verde não aparece em nenhum outro papel.

**Botão secundário**
```
fundo        transparente
borda        1px --iron #2A2421
texto        --bone #D7C4B4
hover        borda --blood, texto --bone-hi
```

**Card**
```
fundo   --pitch #131110
borda   1px --iron #2A2421
raio    0 a 4px  (cartaz não tem canto arredondado)
sombra  0 2px 24px rgba(0,0,0,.6)
```

**Bloco de dor / alerta**
```
fundo   --scab #2A0A09
borda-esquerda  3px --blood
texto   --bone-hi
```

**Destaque de palavra dentro de frase (marca-texto)**
```
fundo --blood, texto #FFFFFF, padding 0 .2em, sem raio, leve rotação (-1deg)
```
Esse é o gesto assinatura do cartaz: a palavra carimbada em bloco vermelho torto.

---

## 6. Textura e forma (o que separa isto de um dark mode qualquer)

A cor sozinha não entrega o clima. Quatro decisões acompanham a paleta:

1. **Grão.** Overlay de ruído sutil (opacidade 3-6%) sobre fundos grandes. Papel impresso,
   não tela OLED. Em web: PNG de ruído tileável ou `feTurbulence` em SVG.
2. **Canto reto.** Raio 0 a 4px no máximo. Cartaz colado em muro não tem borda arredondada.
3. **Desalinhamento intencional.** Rotação de -2 a +2 graus em selos, badges e destaques.
   Perfeição geométrica lê como corporativo, o oposto do que a peça comunica.
4. **Peso tipográfico extremo.** Display em condensed pesado, caixa alta, tracking apertado.
   Corpo em sans neutro leve. O contraste entre os dois é o que grita.

---

## 7. Tipografia

| Papel | Fonte | Peso | Tratamento |
|---|---|---|---|
| Display | Bebas Neue (ou Anton, Oswald 700) | 400/700 | caixa alta, tracking `.02em`, line-height `.95` |
| Corpo | Source Sans 3 (ou Inter) | 400 | line-height `1.6`, sem caixa alta |
| Ênfase | Source Sans 3 | 700 itálico | usar em vez de sublinhado |

---

## 8. Aplicação fora do site

| Meio | Como aplicar |
|---|---|
| Criativo de anúncio | Fundo `--void` com grão, título display em `--bone-hi`, uma palavra carimbada em bloco `--blood`. |
| Thumbnail de vídeo | Rosto dessaturado sobre `--void`, texto `--bone-hi` grande, bloco `--blood` numa palavra só. |
| Slide / apresentação | Fundo `--void`. Um dado por slide em `--bone-hi` gigante. Vermelho só no número que dói. |
| Story / feed | Faixa `--blood` no topo ou base como assinatura recorrente. Repetição cria o reconhecimento. |
| Documento / PDF | Inverte: fundo `--bone`, texto `--void`, títulos `--blood`. Único contexto de inversão permitido. |
| E-mail | Fundo `--bone` (cliente de e-mail quebra dark), CTA em `--blood` com texto branco. |

---

## 9. Proibido

- Vermelho como cor de texto de corpo sobre fundo escuro (`--blood` reprova em contraste).
- Vermelho em texto pequeno, mesmo `--signal`. Abaixo de 24px normal / 18,7px bold, é `--bone-hi`.
- Clarear o vermelho para fazê-lo passar em contraste. Aumente o tamanho, não a luminosidade.
- Gradientes. Serigrafia não tem gradiente.
- Sombra colorida ou glow. Só sombra preta dura.
- Verde, azul ou roxo em qualquer papel semântico. A paleta tem três cores e para em três.
  Única exceção: o verde `#16A34A` do botão de compra (ver seção 5), que existe por
  performance de conversão, não por estética. Não estender esse verde a nada além do CTA.
- `#000000` puro como fundo. Mata o calor do sistema.
- Mais de uma área vermelha grande na mesma tela visível.
- Cantos arredondados acima de 4px.

---

## 10. Bloco de tokens (CSS)

```css
:root {
  /* escuros */
  --void:        #0A0907;
  --pitch:       #131110;
  --ash:         #1C1815;
  --iron:        #2A2421;

  /* vermelhos */
  --blood:       #A61312;
  --blood-deep:  #6E0F0E;
  --blood-hot:   #C4181A;
  --signal:      #E0392A;
  --scab:        #2A0A09;

  /* cremes */
  --bone-hi:     #EFE4D8;
  --bone:        #D7C4B4;
  --bone-mute:   #9C8D82;
  --bone-low:    #857870;
  --bone-dim:    #6B6058;

  /* semânticos */
  --bg:          var(--void);
  --bg-raise:    var(--pitch);
  --text:        var(--bone);
  --text-strong: var(--bone-hi);
  --text-muted:  var(--bone-mute);
  --accent:      var(--blood);
  --accent-text: var(--signal);
  --border:      var(--iron);
}
```

**JSON (para automações, Canva, geradores de criativo)**
```json
{
  "void": "#0A0907", "pitch": "#131110", "ash": "#1C1815", "iron": "#2A2421",
  "blood": "#A61312", "bloodDeep": "#6E0F0E", "bloodHot": "#C4181A",
  "signal": "#E0392A", "scab": "#2A0A09",
  "boneHi": "#EFE4D8", "bone": "#D7C4B4", "boneMute": "#9C8D82",
  "boneLow": "#857870", "boneDim": "#6B6058"
}
```
