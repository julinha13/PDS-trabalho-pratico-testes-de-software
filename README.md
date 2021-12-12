# Trabalho Prático sobre Testes de Software
*Trabalho prático realizado para a disciplina Prática em Desenvolvimento de Software (PDS) ministrada pelo professor Marco Tulio Valente pela UFMG, no semestre 2/2021.*

Nome: Júlia Amorim de Araújo <br>
Matrícula: 2016058344

## [React-dates](https://github.com/airbnb/react-dates)

De acordo com a definição no próprio read.me do github da biblioteca, o react-date é: “An easily internationalizable, accessible, mobile-friendly datepicker library for the web”. Ou seja, é uma biblioteca acessível, internacionalizável e responsiva de calendário web que permite a seleção de datas. A biblioteca pode ser utilizada especialmente em sistemas que utilizam React e está disponível para ser instalada via pacote npm. <br>
Implementada pelo Airbnb é uma biblioteca de código aberto, o código foi desenvolvido utilizando a linguagem JavaScript e para os testes de unidade foi utilizado o framework Jest.

### Exemplos de testes de unidade:

#### Exemplo 1

A biblioteca React-dates implementa uma série de funções úteis que são chamadas em diversas partes do código, dentre elas, a função `isDayVisible` que retorna true ou false para respectivamente, uma data dentro do range de datas exibidas pelo calendário e uma data fora desse range.<br>
A assinatura do método é:

`export default function isDayVisible(day, month, numberOfMonths, enableOutsideDays)`

Um dos testes unitários escritos para verificar esse método é:

```javascript
it('returns true if arg is in visible months', () => {
    const test = moment().add(3, 'months');
    const currentMonth = moment().add(2, 'months');
    expect(isDayVisible(test, currentMonth, 2)).to.equal(true);
});
```

É um teste simples que dado um parâmetro específico verifica se o método `isDayVisible` retorna o valor esperado. No caso, o teste instancia dois valores constantes para serem datas passadas como parâmetro para o método, a primeira data é a data atual (correspondente a quando o teste está sendo executado) mas dois meses a frente. O outro parâmetro passado para o método é o mês atual que é montado de maneira *mockada* como a data atual, mais dois meses, ou seja a data atual porém dois meses no futuro.<br>
Para entender esse teste o ponto principal é entender como o próprio método `isDayVisible` funciona e passar os parâmetros corretos para conseguir verificar e garantir que o resultado está sendo retornado corretamente.


#### Exemplo 2

A biblioteca conta com o componente `DateInput` que renderiza um elemento de input para inserção da data. De maneira mais clara, esse componente é um dos principais para tornar possível a renderização dos inputs conforme figura abaixo:
![image](https://user-images.githubusercontent.com/42300403/145695956-4715e2d5-d01c-4e67-b43c-ebdc32ad7a7f.png)

Um dos testes unitários escritos para esse componente é o seguinte:

```javascript
it('props.displayValue overrides dateString when not null', () => {
    const DATE_STRING = 'foobar';
    const DISPLAY_VALUE = 'display-value';
    const wrapper = shallow(<DateInput id="date" />).dive();
    wrapper.setState({ dateString: DATE_STRING });
    expect(wrapper.find('input').props().value).to.equal(DATE_STRING);
    wrapper.setProps({ displayValue: DISPLAY_VALUE });
    expect(wrapper.find('input').props().value).to.equal(DISPLAY_VALUE);
});
```

Esse teste verifica uma lógica de negócio implementada no componente `DateInput` que determina que o valor da props *value* do *input* (componente HTML) deve ser igual ao valor da props `dateString` caso está seja passada para o componente `DateInput` e a props `displayValue` não esteja definida. Caso seja passado a props `displayValue` para o componente independente do valor de `dateString`, deve-se substituir a props do *input* com o valor do `displayValue`.

Para validar a regra descrita acima, o teste primeiramente define o valor da props dateString do componente DateInput e verifica se o valor da props value do input é igual a props dateString. Logo após, o teste define um valor para a props displayValue e novamente verifica o valor da props value do input, porém, agora validando se esse valor é igual a displayValue, ou seja, garantindo que o valor de displayValue sobrescreve o valor de dateString corretamente.<br>
Esse tipo de testes é importante para garantir a cobertura de branches.


#### Exemplo 3

A biblioteca *React-dates* possui um componente DayPickerNavigation que define a maneira que a navegação do calendário (passar entre as datas, meses e anos) deve ser realizada. Por exemplo, esse componente define se o botão de showNavNextButton, ou botão de próximo deve ou não ser exibido. 

O teste de unidade a seguir verifica a renderização do componente `DayPickerNavigation`.

```javascript
it('renders one button when showNavNextButton is false', () => {
    const wrapper = shallow(<DayPickerNavigation showNavNextButton={false} />).dive();
    expect(wrapper.find('[role="button"]')).to.have.lengthOf(1);
});
```

O teste é bem simples e verifica a regra de renderização do componente baseada na props `showNavNextButton`. O teste basicamente renderiza o componente passando a props `showNavNextButton` como *false* e logo em seguida verifica se o botão de "próximo" não foi renderizado na tela, conforme o esperado.
