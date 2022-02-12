# Criando a Interface do iFood com React Native

Projeto do curso da trilha do Bootcamp da MRV.

Neste projeto criarei a interface de um marcador de hora de banho e tosa de petshop.

O projeto base está [nesta página](https://github.com/pablohdev/app-ifood-clone) do github.

## Parte 1 -  Iniciando o projeto

Utilizaremos o [expo](https://expo.dev) ao invés do cli (?).

No painel do expo eu criei o projeto cloneIfood e fiz a inicialização a partir de `npm i -g expo-cli` e depois `expo init cloneIfood`.

Ao darmos `expo start` nós rodamos o app. Esse comando agora é o início sempre que eu quiser mexer no app. Isso abrirá uma página no navegador onde podemos escolher como acompanhar a execução do nosso código. Eu escolhi acompanhar por uma aba do navegador, ao invés de um simulador de mobile.

O professor orientou mudarmos o caminho "main" que aparece em package.json, criando o AppEntry.js em cloneIfood/src. Esse AppEntry terá o mesmo conteúdo que "IFOOD_CLONE\cloneIfood\node_modules\expo\AppEntry.js". O professor também moveu o script core da aplicação (App.js) para a pasta src.

Instalou o styled-components, o react-navigation/native (core do react-navigation), react-navigation/stack (navegação padrão), react-navigation/bottom-tabs (navegação com abas inferiores). Por estarmos usando o expo, temos que instalar mais algumas dependências a partir dessa linha de código `expo install react-native-screens react-native-safe-area-context` de acordo com a [documentação](https://reactnavigation.org/docs/getting-started/#installing-dependencies-into-an-expo-managed-project) do react-navigation.

## Parte 2

Os códigos que serão exibidos na tela do dispositivo passarão pelo App.js que foi o que nós colocamos na pasta src.

O professor colocou em assets as imagens que iremos utilizar.

Em app.json podemos alterar o backgroundColor: "#F001A" (cor padrão). Em src também teremos um assets, mais precisamente, src/assets/img. Em app.json podemos configurar várias coisas (mas pode dar descompatibilidade com o projeto que está hospedado na plataforma do expo).

Não usei isso => Em src/AppEntry.js colocamos `import 'react-native-gesture-handler';` logo no início de todo o código. Esse import é do react-navigation.

Criamos src/pages, ../Home, renomeamos src/App.js para src/pages/Home/index.js e alteramos nela:

```js
    export default function App(){}

    //para

    export default function Home(){}
```

Criamos src/routes.js:

```js
    import React from 'react';

    import { NavigationContainer } from '@react-navigation/native';
    import { createStackNavigator } from '@react-navigation/stack';

    import Home from './pages/Home';

    const Stack = createStackNavigator();

    const Routes = () => {

        return (
            <NavigationContainer>
                <Stack.Navigator>
                    <Stack.Screen name="Home" component={Home} />
                </Stack.Navigator>
            </NavigationContainer>
        )
    }

    export default Routes
```

Em src/AppEntry.js alteramos para o seguinte:

```js

    //import 'react-native-gesture-handler';
    import registerRootComponent from 'expo/build/launch/registerRootComponent';

    import Routes from './routes';

    registerRootComponent(Routes);

```

## Parte 3

É a mesma da Parte 2. Eles repostaram a parte 2.

## Parte 4

Em Home ele colocou um texto, a imagem de entrada (banner), e dois textos, tudo isso dentro do style criado para a Home. Depois colocou dois botões. Esses botões são importados de src/components/Buttons, que também tem seu style.js.

## Parte 5

Criou a página Principal como cópia de Home inicialmente. Também criou a página Main. Importou a página Main em routes.js e colocou no Stack.Screen. Alterou Stack.Navigator colocando a prop initialRouteName="Main". Ele teve que instalar pelo npm o `react-native-vector-icons` e em Main ele importou o Feather a partir desse módulo. No screenOption do Tab.Navigator ele fez alterações de iconName a partir de ifs comparando routes.name; o screenOption retorna um componente Feather. Ele ainda adicionou a prop tabBarOptions em Tab.Navigator fazendo suas alterações.  

Criou a página Perfil baseada na Principal, fez sua importação em Main e a colocou na Tab.Screen do Tab.Navigator de Main.

## Parte 6

Ele vai consumir uma api para popular o app. A Api está no github do projeto base como "db.json", que é o padrão do servidor de back-end dele, o my-json-server.

Em Principal ele importou os hooks useState (trabalhar com estados) e useEffect (gerenciar eventos no componente, como ele será montado, desmontado, etc) do react, também utilizou alguns do react-native.

O useState retorna dois parâmetros em um array, o estado e o método para manipular este estado.

O useEffect dispara uma função quando algo acontecer. Recebe como parâmetros uma função e recebe quando a função será chamada. Se não colocarmos nenhum parâmetro na segunda posição o useEffect ficará em loop. O useEffect no nosso caso será para consultar a API, e no caso essa consulta será assíncrona em relação à execução do app, portanto temos que usar uma função assíncrona dentro do useEffect. Não podemos declarar como async a função parâmetro (callback) do hook, portanto temos que criar e chamar uma async function buscaDados() dentro do callback. A função buscaDados dá um fetch na api e parseia a resposta para json, passando os atributos deste json para os componentes da tela.

## Parte 7

O retorno do Principal é dependente do estado "loaded", que caso seja false (quando o app inicia) renderiza na tela o ActivityIndicator, que é um loader do react-native; caso true (alterado em useEffect, quando a função busca dados acaba de receber todos os dados) renderiza o texto "Principal" a partir do componente ViewHome(props).

Eu imagino que a função assíncrona nesse caso serve para não deixar com que o app tenha um comportamento lento para o usuário. Enquanto a função busca dados recebe os dados da api (dados esses que podem ser muitos e demorarem para carregar completamente) o app pode executar outras funções, que no caso é exibir um elemento de carregamento.

No style do Principal ele usa o componente Dimensions do reat-native que pega as dimensões do dispositivo que está executando o app. Em Principal ele também importou todos os estilos de style.

SafeAreaView é usado para que o app respeite as bordas do dispositivo.

## Parte 8

Criou o components/RestauranteItem, com seu par index e style. No index é definido o retorno desse componente, com os subcomponentes RestauranteView, RestauranteFoto e RestauranteInfo baseados no style.js. O RestauranteItem recebe parâmetros para ser montado. Ele usou o componente AntDesign importado do @expo/vector-icons. Esse componente renderiza ícones a partir do expo.

Ele importou o RestauranteItem para o Principal e em ViewHome do Principal ele fez um map no estado restaurantes (que é populado no useEffects de Principal, sendo um array de objetos, sendo que cada linha do array é um objeto que representa um restaurante), portanto, cada restaurante gera um RestauranteItem.

## Parte 9

Criou components/CategoriaItem e components/BannerItem, fazendo a importação destes em Principal.

Em Principal, CategoriaView e BannerView são ScrollView importada de style e por isso aceita as propriedades horizontal e showsHorizontalScrollIndicator para definirmos se a disposição dos itens será na horizontal e se aparecerá a barra de rolagem horizontal.

Em routes.js em initialRouteName colocamos "Home".
