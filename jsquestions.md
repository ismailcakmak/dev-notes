### soru 1.

### soru 2 [Solved]

aşağıdaki gibi bir objeyi shallow copy yaptıktan sonra örneğin buna obj2 diyelim. 
```js
const obj = {
    name: "ismail",
    surname: "cakmak",
    age : 22,
    isTeaching: true
    key : {
        name : "thinkbook"
    }
}
```
obj1 komple key objesini yenileyince obj2 de bu değişiklik nedense görünmüyor. 
```js
obj.key = {name:"lenovo"}

// obj2.key still name:"thinkbook"
```
fakat obj.key.name değişince bu değişiklik obj2 ye de yansıyor.

```js
obj.key.name = {name: "lenovo}
//obj1.key.name is now name:"lenovo"
```

bunun sebebi nedir?

Solution :
Sebebi obj.key = {name:"lenovo"} yaptığımızda bunun yeni bir obje oluşurup obj.key in updatelenmesı (yeni bir adrese bakması)dır. bu yüzden obj artık bu yeni adrese bakarken, obj1 eski adrese bakıyor durumunda kalır.

### soru 3
prototype dediğmiz şey nedir tam olarak?
örneğin array classının prototype ı Object classı, 
object classının prototype ı da farklı bir class mı oluyor?
anladığım kadarıyla prototype classın derive edildiği inner class.



### soru 4

here a simple counter code : 

```js
import React from 'react';
import {
  SafeAreaView,
  ScrollView,
  StatusBar,
  StyleSheet,
  Text,
  useColorScheme,
  View,
} from 'react-native';


const styles = StyleSheet.create({
  mainView: {alignItems:"center"}
})



class App extends React.Component{

  constructor(){
    super()
    this.state = {
      counter:0
    }
  }
  
  incrementCounter(){
    this.setState({
      counter: this.state.counter+1
    })
  }

  componentDidMount()
  {
    setInterval(this.incrementCounter.bind(this), 1000)
  }

  render(){
    return(
      <View style={styles.mainView}>
        <Text>{this.state.counter}</Text>
      </View>
    )
  }
  
}

export default App;
```

why this doesnt work :
setInterval(this.incrementCounter, 1000)

cevabı biliyoruz : incrementCounterdaki this objeye bakamıyor

why this doesnt work :
setInterval(this.incrementCounter.bind(this), 1000)

this works :
    setInterval(()=>this.incrementCounter(), 1000)

ben örneğin setIntervala verdiğim fonksiyonun birkaç line code çalıştırmasını
istiyorum diyelim, bunu nasıl yaparım, düz function keyword ile fonksiyon oluşturunca
this bağlanmadan kalmış oluyor, arrow function yapınca sadece return yapabiliyorum.

sanırım cevap : arrow function yapıp içerdeki fonksiyona bu kodu yazmak:

setInterval(()=>this.incrementCounter(), 1000)
increment Counter a bunu yazmak

peki şöyle bir şey oluyor mu : 
setInterval(()=> {line1
this.incrementCounter.bind()}, 1000)



