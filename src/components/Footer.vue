<template>
  <diV class="footer">
    <ul class="footer_list">
      <li class="list_item" @click="switcHomeRouter(index)" v-for="(item, index) in footerList" :key='index'>
        <div class="item-icon"><img :src="footerActiveIndex === index ? item.activePath : item.path" alt=""></div>
        <div class="item-text" :class="{'item-text-active': footerActiveIndex === index}">{{item.name}}</div>
      </li>
    </ul>
  </diV>
</template>
<script lang="ts">
import {Component, Prop, Vue} from 'vue-property-decorator';
@Component
export default class Footer extends Vue {
  // data
  private footerList = [
    {
      name: '首页',
      path: require('@/assets/images/tab_home_nor.png'),
      activePath: require('@/assets/images/tab_home_sct.png')
    },
    {
      name: '方案',
      path: require('@/assets/images/tab_icon_design_nor.png'),
      activePath: require('@/assets/images/tab_icon_project_sct.png')
    },
    {
      name: '设计我家',
      path: require('@/assets/images/tab_icon_design.png'),
      activePath: require('@/assets/images/tab_icon_design_sct.png')
    },
    {
      name: '同城服务',
      path: require('@/assets/images/tab_icon_fuwu_nor.png'),
      activePath: require('@/assets/images/tab_icon_fuwu_sct.png')
    },
    {
      name: '我的',
      path: require('@/assets/images/tab_icon_me_nor.png'),
      activePath: require('@/assets/images/tab_icon_me_sct.png')
    }
  ]
  private footerActiveIndex = 0
  // created
  created() {
    interface labelValue {
      color: string;
      age: number;
      height?: string;
      readonly weight: number;
    }
    let point:labelValue = {color: '1', age: 24, weight: 1}
    let poinTwo: {readonly color: string, age?: number} = {color: '1', age:2}
    poinTwo.age = 4
    console.log(poinTwo, 'poinTwo')
    // interface funValue {
    //   color?: string,
    //   area?: number
    // }
    // function createSquare(config: funValue): { color: string; area: number } {
    //     // ...
    // }
    interface SquareConfig {
        color: string;
        width: number;
        [propName: string]: any;
    }
    function createSquare(config: SquareConfig): { color: string; area?: number } {
      console.log(config.colorur)
      return {color: config.color, area: config.width}
    }
    let a = { color: 'red', width: 100, colorur: 1 }
    let mySquare = createSquare(a);
    // 函数类型的接口
    interface functionType {
      (type: string, count: number): boolean
    }
    let myTest:functionType = (a: string, b: number) => {
      let flag:boolean = b === 1
      return flag
    }
    console.log(myTest('1', 1))
    // 可索引类型
    // (1)数组
    interface testArray {
      [index: number]: string
    } // 表示一个string类型的数组
    let arr: testArray = ['1','2']
    let arrTwo: number[] = [1,2]
    // (2)对象
    interface testObject {
      [name: string]: number
    }
    let obj: testObject = {a: 1, b: 2}
    // (3)同时使用两种索引
    interface testArrObj{
      [index: number]: string;
      [name: string]: string;
    }
    let arrObj: testArrObj = {0: '11', a: '22'}
    console.log(arrObj, 'arrObj')
    // (4)类型与索引类型返回值的类型不匹配
    interface NumberDictionary {
      [index: string]: number;
      length: number;    // 可以，length是number类型
      name: number       // 错误，`name`的类型与索引类型返回值的类型不匹配
    }
    // (5)将索引设置只读
    interface ReadonlyStringArray {
     readonly [index: string]: number
    }
    let one:Array<object> = [{a:1}]
    let two: object = {a: 1, b: "2"}
    // 类类型
    // (1)实现接口
    interface classParams {
      currentData: string;
      setCurrentData (d: string): void;
    }
    class myClass implements classParams {
      constructor(public currentData: string) { // 这种写法是ts的简写，可以直接用this.currentData访问到
      }
      setCurrentData(d: string):void {
        this.currentData = d
        console.log(this.currentData, 'swqwq')
      }
    }
    let classTest = new myClass('1')
    classTest.setCurrentData('3')
    // (2)类的静态部分跟实例部分的区别
    interface classDependency {
      setDate() : void
    }
    interface classInterface {
      new (a: number, b: string) : classDependency
    }
    let classTestFunc = (cla: classInterface, a: number, b: string): any => {
      return new cla(a, b)
    }
    class dog implements classDependency {
      constructor(public myClassParams: number, b: string) {}
      setDate():void {
        console.log(this.myClassParams, '哈哈')
      }
    }
    classTestFunc(dog, 10000000, '1').setDate()
    // 继承接口
    // (1)一个接口的继承
    interface Shape { age: number }
    interface ShapeTwo extends Shape { height: string }
    let ShapeTwo: ShapeTwo = {age: 1, height: '2'}
    // (2)两个接口的继承
    interface MoreOne { age: number}
    interface MoreTwo { color: string}
    interface MoreThree extends MoreOne, MoreTwo { obj: object}
    let moreThree:MoreThree = {age: 1, color: '1', obj: {a: 1}}
    // (3)对象赋值的另外一种方式
    interface Assignment { age:number }
    let assignment = <Assignment>{}
    assignment.age = 1
    // (4)多种类型
    let testType: number | string = 1
    interface testType {
      age: number;
      color: number;
      height: string;
    }
    let commomType = <testType>{
      age: 1,
      color: 1,
      height: '1'
    }
    interface MixtureType {
      setDate():void;
      (start: number) :void;
      color: string;
    }
    function mixtureType (a:number, b: string): MixtureType {
      let MixtureType = <MixtureType>function (start:number) { console.log( MixtureType, 'MixtureType' )}
      MixtureType.setDate = () => {}
      MixtureType.color = b
      return MixtureType
    }
    console.log(mixtureType(1,'2')(1), 'aaa')
    // 接口继承类
    class Contro {
      public type: any;
    }
    interface ControInterface extends Contro{
      setDate():void
    }
    class Button extends Contro implements ControInterface {
      setDate() {}
    }
    class Image implements ControInterface{ // 因为Image不是Contro的子类，所以没办法访问私有属性
      constructor(public type:any) {}
      setDate() {}
    }
    // 类
    // (1)公有修饰符
    class Animate {
      public type = 1;
      public move (type: number) {
        // this.type = type
      }
    }
    let animate = new Animate()
    animate.move(2)
    console.log(animate.type, 'swqqw')
    // (2)私有修饰符
    class AnimateTwo {
      private type = 1;
    }
    // console.log(new AnimateTwo().type)
    // (3)protected修饰符
    class AnimateThree {
      protected type = 1
    }
    class AnimateFour extends AnimateThree{
      public typeTwo = 1
      setDate() {
        this.typeTwo = this.type + 10 // 可以在子类中被访问
      }
    }
    let animateFour = new AnimateFour()
    animateFour.setDate()
    // console.log(animateFour.typeTwo ,'可以访问')
    // console.log(animateFour.type, "不可以访问")
    // (4)readonly修饰符
    class ReadonlyOne {
      readonly type: number = 1;
      public text: number = 1;
      test(type: number) {
        // this.type = 2 // name是只读的
      }
    }
    let readonlyOne = new ReadonlyOne()
    // readonlyOne.type = 3 // name 是只读的
    // (5)存取器
    interface FullNameInterface {
      _fullName: string;
      setDate(): void;
      sexList: Array<string>;
    }
    class FullNameText implements FullNameInterface{
      public _fullName: string = '孙';
      public sexList: Array<string> = ['赵','钱','孙','李'];
      setDate():void {}
      set fullName(newName: string) { // 设置值
        if (this.sexList.find(n => n === newName)) {
          this._fullName = newName
        } else {
          console.error(`对不起，没有${newName}这个姓氏`)
        }
      }
      get fullName():string { // 获取值
        return this._fullName + '卫其'
      }
    }
    let fullNameText = new FullNameText()
    fullNameText.fullName = '加特' // 等号右边，表示传递给set的参数
    console.log(fullNameText.fullName, 'fullName') // 表示获取
    fullNameText.fullName = '赵'
    console.log(fullNameText.fullName, 'fullName')
    // 4、静态属性
    class StaticText {
      static origin:object = {x:1, y:2} // 不是在实例里面而是在函数本身
      setData():void {
        console.log(StaticText.origin, 'swq')
      }
    }
    let staticText = new StaticText()
    console.log(StaticText.origin, 'swqwq')
    // 5、抽象类
    abstract class AbstractText {
      setData() {
        console.log(1)
      }
      abstract setName():void;
    }
    class AbstractTextChild extends AbstractText {
      setData() {
        console.log(2)
      }
      setName():void{
        console.log(3)
      }
    }
    new AbstractTextChild().setData()
    // 函数类型
    // 1、为函数定义类型
    function add(x:number, y:number): number { return x + y }
    let myAdd:(x:number, y: string) => string = function(a:number, b: string):string {return a + b}
    let myAddTwo: (x:number, y: string) => string = function(a, b) {return a + b}
    let myAddThree = function (a: number, b: string):string {return a + b}
    let myAddFour = function ():void { console.log(1)}
    // myAddFour(1,2) // 报错
    let myAddFive = function(a:string, b: number):string { return a + b }
    // myAddFive(1,2,3)
    let myAddSix = function(a?: number, b: string):string { return a + b} 
    let myAddSeven = function(a:number,b?:number, c :number):number { return a}
    let myAddEight = function(a: number, b = "a") : string { return a + b}
    myAddEight(1, undefined)
    // myAddEight(1, 2)
    let myAddNice = function (a = "a", b: number, c = "c", d:string, e = "e"): string { return a + b}
    function myAddTen (a: number, ...arr: number[]) : number { console.log(arr);return a + 10 }
    myAddTen(1,2,3,4,5,6)
    let afsdf:number[] = [12,2332]
    let myAddEleven: (a: number, ...test: number[]) => Array<number> = 
    function (a: number, ...test: number[]) : Array<number> { return test.concat([a])}
    console.log(myAddEleven(1,2,3,4,5,6,6), 'myAddEleven')
    // function myAddTwelve(a:number, ...test: number[], c: string):void {}
    interface MyAddTwelve {
      setData(this: object):void
    }
    let myAddTwelve: MyAddTwelve = {
      setData(this: void) {
        console.log(this, 'this')
      }
    }
    document.addEventListener('click', myAddTwelve.setData)
    function myAddThirteen(x:number):number // 重载1
    function myAddThirteen(x:{a: number, b: string}):string // 重载2
    function myAddThirteen(x: any):any {
      if (typeof x == 'object') {
        return '1'
      } else if (typeof x == 'number') {
        return x
      }
    }
    myAddThirteen({a: 1, b: '1'})
    // 泛型
    function myAddFourteen<X> (x: X) : X{ return x }
    function myAddFiveteen(x: any):any { return x + 1}
    myAddFourteen<string>('sunweiqi')
    // 泛型变量
    function myAddSixteen<T>(x: Array<T>) { console.log(x.length); return x}
    // 泛型类型
    interface MyAddSeventeen<T> {
      <T>(x:T): T
    }
    let myAddSeventeen: MyAddSeventeen<number> = <T>(x:T):T => { return x }
    // 泛型类
    class MyAddEighteen<T> {
      constructor(public value:T) {}
      setData(x: T):T { return x}
    }
    let myAddEighteen = new MyAddEighteen<string>('mySell')
    // 泛型约束
    interface MyGenericityConstraint {
      length: number
    }
    function myGenericityConstraint <T extends MyGenericityConstraint>(x: T) {
      console.log(x.length) // 本来这里是会报错的，因为泛型不知道你传进来的时候有没有length，但是上面做了泛型约束，所以就可以
    }
    // 在泛型中使用类型参数
    // function myGenericityOne <T, K>(x: T, y: K) {
    //   return x[y]
    // }
    // 1\数字枚举
    // (1)使用初始值
    enum myEnumOne {
      one = 0,
      two = 2,
      three = 9,
      four // 这个就是10
    }
    console.log(myEnumOne)
    // (2)不适用初始值
    enum myEnumTwo {
      one, // 这个默认就是0，其余规则跟有初始值一致
      two = 10,
      three,
      four = 11,
      five
    }
    console.log(myEnumTwo, 'myEnumTwo')
    // 2字符串枚举
    enum myEnumThree {
      one = 'one',
      two = 'two',
      three ='three', // 必须有值，不然会报错
    }
    console.log(myEnumThree, 'myEnumThree')
    // 3、常量成员
    enum myEnumFour {
      one,
      two = 1 << 1,
      three = 1 << 2,
      four = one | two,
      G = '123'.length
    }
    console.log(myEnumFour, 'myEnumFour')
    // 4、枚举成员的值
    enum myEnumFive {
      one,
      two
    }
    interface MyEnumFive {
      one: myEnumFive.one,
      two: number
    }
    let myEnumFiveTest : MyEnumFive = {
      one: myEnumFive.one, // 这个地方写myEnumFive.one，写任何number类型都OK
      two: 2
    }
    console.log(myEnumFiveTest, 'myEnumFiveTest')
    // 运行时的枚举
    enum myEnumSix { X, Y, Z }
    let myEnumSixFunc = <T>(obj: {X: T}):T => { return obj.X}
    console.log(<number>myEnumSixFunc(myEnumSix), 'myEnumSix') // 0
    // 反向影射
    enum myEnumSeven { X }
    let X = myEnumSeven.X
    console.log(myEnumSeven[X]) // 'X'
    // 外部枚举
    // declare enum myEnumEight {
    //   A = 1,B,C
    // }
    // 类型推论
    let zoo = [1, {a: 1}, [1,2,3]] // zoo的类型就是(number | object | Array<number>)
    // 类型兼容性
    interface Named {
      name: string
    }
    class Person {
      name: string = '1'
    }
    let P:Named = new Person()
    // 1、a兼容b
    interface a { name: number}
    let B = { name: 1, age: ''}
    let A:a = B // 不会报错，只要B包含a接口的所有属性
    // 2、兼容性体现在函数传参中
    let myTypeOne = (a: a) => { return a.name }
    myTypeOne(B)
    // 3、函数的比较
    let myTypeTwo = (a: number) => {}
    let myTypeThree = (a: number, b: number) => {}
    myTypeThree = myTypeTwo
    let myTypeFour:(a: number, b: number) => number = (a: number):number => { return a} // 正常
    // let myTypeFour:(a: number) => number = (a: number, b: number):number => { return a} // 报错
    let myTypeFive = () => { return { name: 'name', age: 'age'}}
    let myTypeSix = () => { return { name: 'name'}}
    myTypeSix = myTypeFive // 正确
    // myTypeFive = myTypeSix // ERROR
    // 可选参数及剩余函数
    let myTypeSeven: (a: number, b: number) => object = (a:number, b:number, c?:string) => { return {}}
    let myTypeEight: (a: number, b?: number) => object = (a:  number) => {return {}}
    let myTypeNice:(a: number, b?: number) => object = (a: number, b?:number, c?:string) => {return {}}
    // 枚举之间的类型是不兼容的
    enum myTypeTeen { a,b,c}
    enum myTypeEleven { a,b,c}
    let MyTypeTeen = myTypeTeen.a
    // MyTypeTeen = myTypeEleven.a // 报错，因为已经是myTypeTeen类型赋值过的，不能再用myTypEleven赋值
    class MyTypeTwelve {
      // str:string = '1'
      constructor(a:number) {}
    }
    class MyTypeThirteen {
      // str:string = '1'
      constructor(a:number) {}
    }
    let Twe:MyTypeTwelve 
    let Thi:MyTypeThirteen 
    // Twe = Thi
    // 泛型
    interface Empty<T> {
      a: T // 如果没有这个成员，就是OK的，如果有这个成员就是Error
    }
    let empty:Empty<string> = {a: '1'}
    let emptyTwo:Empty<number> = {a: 1}
    // empty = emptyTwo
    // 高级类型
    // 1、交叉类型
    let extent = <T,U>(first: T, second: U): (T & U) => {
      let result = <T & U>{}
      for (let key in first) {
        (<any>result)[key] = first[key] // 必须用类型断言，不然会报错
      }
      for (let key in second) {
        (<any>result)[key] = second[key] // 必须用类型断言，不然会报错
      }
      return result
    }
    class myAdvancedFirst {
      public feet: string = '1'
      public width: string = '2'
    }
    class myAdvancedSecond {
      public width: string = '2'
      constructor(public person:number = 1) {}
    }
    let sun = extent(new myAdvancedFirst(), new myAdvancedSecond())
    console.log(sun, 'sun') // {feet: "1", person: 1}
    // 2、联合类型
    // (1)普通使用方式
    let myAdvanceThree: number | string | object = 1
    // (2)高级方式
    interface MyAdvancedFirst { logs():void, setData(): object }
    interface MyAdvancedSecond { logs():void , setType(): object}
    let myAdvanceFour: (MyAdvancedFirst | MyAdvancedSecond) = {
      logs(){
        console.log('logs')
      },
      setData() {
        return {}
      },
      setType(){
        console.log('setType')
      }
    }
    console.log((<MyAdvancedFirst>myAdvanceFour).setData(), 'setData') // OK,使用了类型断言
    // 类型谓词is
    let myAdvancedFive= (pet: MyAdvancedFirst |  ): pet is MyAdvancedFirst => {
      return (<MyAdvancedFirst>pet).logs !== undefined
    }
    console.log(myAdvancedFive({logs() {}, setType() {return {}}}), 's')
    // typeof类型保护
    let myAdvancedSix = (value: string | number , padding: string) => {
      if (typeof value === 'number') {
        return Array(value + 1).join(' ') + value
      } else if (typeof value === 'string') {
        return value + padding
      }
    }
    /**************************************隔开**********************************************/
    class aa {
      public name: string
      constructor(str: string, public a: number) {
        this.name = str
      }
    }

    class bb extends aa {
      constructor() {
        super('1', 2)
      }
    }
  }
  // methods
  private switcHomeRouter (index: number) {
    this.footerActiveIndex = index
  }

}
</script>
<style scoped lang='scss'>
  .footer{
    width: 100%;
    height: 88px;
    position: fixed;
    left: 0;
    bottom: 0;
    .footer_list{
      width: 100%;
      height: 100%;
      display: flex;
      .list_item{
        flex: 1;
        .item-icon{
          img{
            width: 40px;
            height: 40px;
          }
        }
        .item-text{
          text-align: center;
          height: 30px;
        }
        .item-text-active{
          color: #29cccc;
        }
      }
    }
  }
</style>


