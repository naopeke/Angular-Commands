# Angular Commands
```
npm i -g @angular/cli@16.2.10
```
```
npm uninstall -g @angular/cli
```
```
npm cache clean --force
```
```
ng new my-app(nombre de aplicacion)</p>
```

**MODULOS CON CLI**  
モジュールの作成
```
ng generate module nombre-del-modulo
ng g m nombre-del modulo
```

**MODULOS**
```
import{ NgModule } from '@angular/core';
import { CommonModule } from '@angular/common'
import { MiComponente } from './mi-componente.component'

@NgModule({<br>
    declarations: [MiComponente],
    imports: [CommonModule],
    exports: [MiComponente]
})

export class MiModulo {}
```

**COMPONENTS CON CLI**  
コンポーネントの作成
```
ng generate component nombre-del componente
ng g c nombre-del componente
```

**4 archivos**
__.component.ts<br>
__.component.html<br>
__.component.css<br>
__.component.spec.ts<br>

```
npm start<br>
```
**Actively supported versions**
https://angular.io/guide/versions
Node.js
```
nvm install 18.10.0 // to install the version of node.js I wanted
nvm use 18.10.0  // use the installed version
```
Angular (Downgrade @angular-devkit/build-angular)
```
npm list @angular-devkit/build-angular
npm install @angular-devkit/build-angular@16.2.10 --save-dev
```

http://localhost:4200/

**親のコンポーネント作成（myApp-src-app内トップ）**
```
ng g c padre
```
```
app.module.ts
@NgModule({
    declarations: [
        AppComponent,
        PadreComponent
        // added automatically
    ]
})
```

**INPUT**  
親コンポーネントから子コンポーネントに値を引き渡す   
https://qiita.com/masaks/items/677195b78379e0877e24
```
//Componente hijo
@Input() datoEntrada: string;

//Componente padre
<app-hijo [datoEntrada]="valorDesdePadre"></app-hijo>

//Componente padre
valorDesdePadre = "Hola, mundo!"

//Componente hijo template
{{ datoEntrada }}
```

**OUTPUT**  
 子コンポーネントから親コンポーネントにイベント(値)を引き渡す
```
//Componente hijo
@Output() messageEvent = new EventEmitter<string>();
message: string = '';

sendMessage(){
    this.messageEvent.emit(this.message)
}

//Componente hijo
<div>
<label for="childInput">Mensaje:</label>
<input id="childInput"> [(ngModel)]="message" />
<button (click)="sendMessage()">Enviar Mensaje</button>

//Componente padre
receivedMessage: string = '';

receiveMessage(message: string){
    this.receivedMessage = message;
}

//Componente padre
<div><br>
<app-child (messageEvent)="receiveMessage($event)"></app-child>
<p>Mensaje recibido en el padre {{ receivedMessage}}</p>
```

**Servicio con Cli**  
サービスとは、アプリケーションのロジックやデータを扱うためのクラスです。  
Angularでは基本的に、コンポーネントが画面の表示を担当し、サービスがその他の処理を担当するように設計します。  
https://tech.quartetcom.co.jp/2023/02/28/angular-service-providing-guide/
```
ng generate service nombre-del-servicio
ng g s nombre-del-servicio
```
```
@import { Injectable } from '@anglar/core';

@Injectable({
    providedIn: 'root'
})

export class MiServicioService {
    constructor() {}

      //Métodos y lógica del servicio
}
```
**Inyeccion de dependencias**   
依存性の注入  Inject関数  
https://zenn.dev/tkawa01/articles/d7245446018de0  
https://zenn.dev/lacolaco/articles/why-inject-function-wins  
https://angular.jp/guide/dependency-injection  
https://angular.jp/guide/dependency-injection-in-action
```
import { Component } from '@anglar/core';
import { MiServicio } from './mi-servicio.service';

@Component({
    selector: 'app-mi-componente',
    templateUrl: './mi-componente.component.html'
})
export class MiComponente {
    constructor(privete miServicio: MiServicio){
        //una variable, miServicio e inyecta esta forma de MiServicio
        //...
        //a partir de Angular17, "inject". servicio inject MiServicio<br>
    }
}
```

**Directiva**  
https://zenn.dev/knts0/articles/8afe1dd9d8981daf8286
ディレクティブ
```
ng generate directive nombre-de-la-directiva
ng g d nombre-de-la-directiva
```
```
//html
<div appMiDirectiva>
 Este es un elemento con mi directiva personalizada.
 </div>

//nombre-de-la-directiva.directive.ts
import { Directive, ElementRef } from '@angular/core';

@Directive({
    selector: '[appMiDirectiva]'
})
export class MiDirectivaDirective {
    constructor(private el: ElementRef){
        //Accede al elemento del DO en el que se aplica la directiva (this.el.nativeElement)
        this.el.nativeElement.style.backgroundColor = 'yellow';
    }
}
```

**PIPES**
```
ng generate pipe nombre-del-pipe
ng g p nombre-del-pipe
```
```
//nombre-del-pipe.pipe.ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
    name: 'miPipe'
})
export class MiPipe implements PipeTransform {
    transform(valor: any): any{
        //Implementa la lógica de transformación aquí
        return valor.toUpperCase();
    }
}
```
```
<p>{{texto | miPipe }}</p>
https://angular.io/guide/pipes
```

**Rutas(Routes)**
```
<p>const routes: Routes = [
    { path: 'inicio', component: InicioComponent },
    { path: 'productos', component: ProductosComponent },
    { path: 'contacto', component: ContactoComponent },
]</p>
```

**Router Outlet**
```
<router-outlet></router-outlet>
```

**Navegación**
```
<a routerLink="/inicio">Inicio</a>
```

**Parámetros de Ruta**
```
{ path: 'producto/:id', component: DetalleProductoComponent }
<a [routerLink]="['/producto', producto.id]">Ver Detalles</a>
```

**routerLinkActive**
```
<nav>
<a routerLink="/inicio" routerLinkActive="active">Inicio</a>
<a routerLink="/productos" routerLinkActive="active">Productos</a>
<a routerLink="/contacto" routerLinkActive="active">Contacto</a>
</nav>
```

**Parámetros por la URL**
1. Definir una ruta con varios parámetros:
```
const routes: Routes = [
    { path: 'producto/:categoria/:id', component: DetalleProductoComponent },
];
```

2. Enlazar a la ruta con múltiples parámetros:
```
<a [routerLink]="['/producto', producto.categoria, producto.id]">Ver Detalles</a>
```

3. Recuperar los parámetros en el componente:
```
import { ActivatedRoute } from '@angular/router';

//...

constructor(private route: ActivatedRoute) {}

ngOnInit(){
    this.route.params.subscribe(params => {
        const categoria = params['categoria'];
        const productId = params['id'];
        // Hacer algo con los valores de los parámetros
    })
}
```

**Navegar desde el controller**
```
import { Router } from '@angular/router';

//...

constructor(private router: Router) {}

//...

navegarAProducto(productoId: number){
    //Puedes navegar a una ruta especifica programáticamente
    this.router.navigate(['/producto, productoId]);

}
```

**Bootstrap**
```
ng new landing-page-angular
```
```
npm i bootstrap@5.3.2
```

ルーティングモジュール(app-routing.module.ts)を手動で追加:
```
ng generate module app-routing --flat --module=app
```
あるいは  
新しいプロジェクトを--routingフラグをつけて作成
```
ng new my-app --routing
```

node_module - dist - css - bootstrap.min.css - (click) copy relative path 
=> (paste)angular.json 
```
//27, 92
"styles": [
    "src/styles.css",
    "node_modules/bootstrap/dist/css/bootstrap.min.css"
]
```
node_module - dist - js - bootstrap.bundle.min.js - (click) copy relative path
```
//30, 95
    "scripts": [
    "node_modules/bootstrap/dist/js/bootstrap.bundle.min.js"
    ]
```
**Crear otros componentes**
```
ng g c home
```
```
ng g c products
```
```
ng g c contact
```
```
ng g c product-detail
```

**Estructuras de Control**  
**ngIf**
```
<div *ngIf = "mostrarElemento">
 Contenido visible si mostrarElemento es true.
</div>
```
**ngFor**
```
<ul>
<li *ngFor="let item of listaItems">
{{ item }}
</li>
</ul>
```
**ngSwitch**
```
<div [ngSwitch]="opcion">
 <p *ngSwitchCase="'opcion1'">Contenido para opción 1</p>
 <p *ngSwitchCase="'opcion2'">Contenido para opción 2</p>
 <p *ngSwitchDefault>Contenido por defecto</p>
</div>
```
**ngClass**
```
<div [ngClass]="{'clase1': condicion1, 'clase2': condicion2}">
  //Contenido con clases dinámicos
</div>
```
**ngStyle**
```
<div [ngStyle]="{'color': color, 'font-size': tamano}">
  //Contenido con estilos dinámicos
</div>
```
**ngContainer**
```
<ng-container *ngIf="condicion">
  //Contenido que no afecta al DOM directamente
</ng-container>
```
**otras estructuras de control:**  
ngTemplate  
ngPlural  
ngComponentOutlet  

**Formulario**
Importar FormsModule para formularios de plantilla
```
import { FormsModule } from '@angular/forms';

@NgModule({
    declarations: [
        //tus componentes aquí
    ],
    imports: [
        FormsModule,
        // otros modulos que estés utilizando
    ],
    bootstrap: [AppComponent],
})
export class AppModule {}
```
**Formularios basados en plantillas (Template-driven);**
```
<form #myForm="ngForm" (ngSubmit)="onSubmit()">
<label for ="name">Nombre:</label>
<input type="text" id="name" name="name" [(ngModel)]="user.name" required>

<label for ="email">Correo:</label>
<input type="email" id="email" name="email" [(ngModel)]="user.email" required>

<button type="submit">Enviar</button>
</form>
```
**Manejo de estado y errores**
```
<div *ngIf="!name.valid && name.touched">Nombre es obligatorio.</div>
```
**Importar ReactiveFormsModule para formularios reactivos**
```
import { FormsModule } from '@angular/forms';

@NgModule({
    declarations: [
        // tus componentes aquí
    ],
    imports: [
        ReactiveFormsModule
        // otros módulos que estés utilizando
    ],
    bootstrap: [AppComponent],
})
export class AppModule {}
```
**Formularios reactivoe (Reactive): utiliza el servicio FormBuilder**
HTML
```
<form [formGroup]="myForm" (ngSubmit)="onSubmit()">
<label for="name">Nombre:</label>
<input type="text" id="name" formControlName="name">

<label for="email">Correo:</label>
<input type="email" id="email" formControlName="email">

<button type="submit">Enviar</button>
```
TS
```
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

constructor(private fb: FormBuilder){
    this.myForm = this.fb.group( {
        name: ['', Validators.required],
        email: ['', [Validators.required, Validators.email]],
    });
}
```
**Manejo de estado y errores**
```
<div *ngIf="myForm.get('name').hasError('required') && myForm.get('name').touched">
 Nombre es obligatorio.
</div>
<div *ngIf="myForm.get('email').hasError('email') && myForm.get('email').touched">
 Correo no válido.
</div>
```
**Ciculo de vida**  
1.ngOnChanges  
1.ngOnInit  
1.ngDoCheck  
1.ngAfterContentInit  
1.ngAfterContentChecked  
1.ngAfterViewInit  
1.ngAfterViewChecked  
1.ngOnDestroy  

**ngOnChanges**
nombreInputというプロパティがあり、親コンポーネントから変わった際に、ngOnChangesが動く。  
ngOnChangeの中からは、パラメータchangesからその変更にアクセスでき、その変更を実行できる。  
```
import { Component, Input, OnChanges, SimpleChanges } from '@angular/core';

@Component({
    selector: 'app-mi-componente',
    template: '<p>Hola, {{ nombre }}</p>
})
export class MiComponente implements OnChanges {

    //Propiedad de entrada
    @Input() nombreInput:string;

    //Propiedad del componente
    nombre:string;

    //Método ngOnChanges se llama cuando hay cambios en las propiedades de entrada
    ngOnChanges(changes: SimpleChanges):void{
        //Verifica si la propiedad de entrada 'nombreInput' ha cambiado
        if(changes.nombreInput){
            this.nombre = changes.nombreInput.currentValue;
            console.log(`Se ha cambiado el valor de nombreInput a: ${this.nombre}`);
        }
    }
}

```
**ngOnInit**
ngOnInitはすべてのコンポーネントのプロパティを開始したあとに実行する。
```
import { Component, OnInit } from '@angular/core';

@Component({
    selector: 'app-mi-componente',
    template: '<p>Hola, mundo!</p>',
})
export class MiComponente implements OnInit {

    //Propiedad de ejemplo
    nombre: string;

    //Constructor del componente
    constructor(){
        //Puedes incializar propiedades aquí, pero es una buena práctica hacerlo en ngOnInit
    }

    //Método ngOnInit se llama después de que Angular ha inicializado todas las propiedades del componente
    ngOnInit(): void {
        this.nombre = 'Usuario';
        console.log(`Hola, ${this.nombre}! El componente se ha inicializado`);
    }
}
```
**ngDoCheck**
Angular内での変更を見つける。その度に発動するので複雑なものは入れないこと。
```
import { Component, DoCheck } from '@angular/core';

@Component({
    selector: 'app-mi-componente',
    template: '<p>Hola, {{ nombre }}!</p>',
})
export class MiComponente implements DoCheck {
}
    nombre:string = 'Usuario';

    //Método ngDoCheck se llama durante cada detección de cambios
    ngDoCheck():void {
        console.log('Se está ejecutando ngDoCheck');
        //Puedes realizar acciones de verificación personalizadas aquí
    }
```
**ngAfterContentInit**
```
import { Component, AfterContentInit, ContentChild, ElementRef } from '@angular/core';

@Component({
    selector: 'app-mi-componente',
    template: '<ng-content></ng-content>',
})
export class MiComponente implements AfterContentInit {

    //ContentChild para acceder a un elemento dentro del contenido proyectado
    @ContentChild('nombreElemento') nombreElemento: ElementRef;

    //Método ngAfterContentInit se llama después de que Angular haya proyectado el contenido
    ngAfterContentInit():void{
        //Realizar acciones después de que el contenido haya sido inicializado
        if(this.nombreElemento){
            console.log(`Se ha encontrado un elemento con el nombre 'nombreElemento'.`);
        }
    }
}
```
**ngAfterContentChecked**
```
import { Component, AfterContentChecked, ContentChild, ElementRef } from '@angular/core';

@Component({
    selector: 'app-mi-componente',
    template: '<ng-content></ng-content>',
})
export class MiComponente implements AfterContentChecked {

    //ContentChild para acceder a un elemento dentro del contenido proyectado
    @ContentChild('nombreElemento') nombreElemento: ElementRef;

    //Método ngAfterContentChecked se llama después de cada verificación del contenido proyectado
    ngAfterContentChecked():void {
        //Realizar acciones después de cada verificación del contenido
        if (this.nombreElemento){
            console.log(`Se ha verificado el contenido y se ha encontrado un elemento con el nombre 'nombreElemento'.`);
        }
    }
```
**ngAfterViewInit**
```
import { Component, AfterViewInit, ViewChild, ElementRef } from '@angular/core';

@Component({
    selector: 'app-mi-componente',
    template: '<p #miParrafo>Hola, mundo!</p>,
})
export class MiComponente implements AfterViewInit {

    //ViewChild para acceder a un elemento en la vista del componente
    @ViewChild('miParrafo') miParrafo: ElementRef;

    //Método ngAfterViewInit se llama después de que Angular haya inicializado las vistas del componente
    ngAfterViewInit():void {
        //Realizar acciones después de que la vista haya sido inicializada
        if (this.miParrafo){
            console.log(`Se ha inicializado la vista y se ha encontrado un párrafo: ${this.miParrafo.nativeElement.textContent}`);
        }
    }
}
```
**ngAfterViewChecked**
```
import { Component, AfterViewChecked, ViewChild, ElementRef } from '@angular/core';

@Component({
    selector: 'app-mi-componente',
    template: '<p #miParrafo>{{ mensaje }}</p>,
})
export class MiComponente implements AfterViewChecked {

    mensaje: string = 'Hola, mundo!';

    //ViewChild para acceder a un elemento en la vista del componente
    @ViewChild('miParrafo') miParrafo: ElementRef;

    //Método ngAfterViewChecked se llama después de cada verificación de las vistas del componente
    ngAfterViewChecked():void {
        //Realizar acciones después de cada verificación de las vistas
        if(this.miParrafo){
            console.log(`Se ha verificado la vista y el contenido del párrafo es: ${this.miParrafo.nativeElement.textContent}`);
        }
    }

}
```
**ngOnDestroy**
```
import { Component, OnDestroy } from '@angular/core';
import { Subscription } from 'rxjs';

@Component({
    selector: 'app-mi-componente',
    template: '<p>Adiós, mundo!</p>,
})
export class MiComponente implements OnDestroy {

    //Ejemplo de suscripción a un observable
    private subscription: Subscription;

    constructor(){
        //Simulación de suscripción a un observable
        this.subscription = new Subscription();
    }

    //Método ngOnDestroy se llama justo antes de que Angular destruya el componente
    ngOnDestroy():void{
        //Realizar limpieza, como desuscribirse de observables o liberar recursos
        if (this.subscription){
            this.subscription.unsubscribe();
            console.log('Se ha desuscrito de la suscripción en ngOnDestroy');
        }
    }
}
```
# API
API  : Application Programing Interface   
Rest : REpresentational State Transfer  
REST API は、REST アーキテクチャの制約に従って、RESTful Web サービスとの対話を可能にする アプリケーション・プログラミング・インタフェース (API または Web API) です。REST (Representational State Transfer) は、コンピュータ・サイエンティストの Roy Fielding によって作成された API の構築方法を定義する仕様であり、REST 用に設計された REST API (または RESTful API) は軽量で高速であるため、IoT (モノのインターネット)、モバイル・アプリケーション開発、サーバーレス・コンピューティングなどの先進的なコンテキストに最適です。  
https://www.redhat.com/ja/topics/api/what-is-a-rest-api  
HttpClientModuleをインポート  
(NgModule APIとは）https://angular.jp/guide/ngmodule-api
```
import { HttpClientModule } from '@angular/common/http';

@NgModule({
    declarations: [
    //tus componentes aquí
],
    imports:[
     HttpClientModule,
    //otros módulos aquí
],
bootstrap: [AppComponent]
})
export class AppModule { }
```
**使い方**
```
ng generate service my-api
```
```
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
 providedIn: 'root'
})
export class MyApiService {
 private apiUrl = 'https://mi-api.com';

constructor(private http: HttpClient) { }

getData(): Observable <any> {
 return this.http.get(`${this.apiUrl}/datos`);
}

postData(data:any): Observable<any> {
 return this.http.post(`${this.apiUrl}/enviar-datos`, data);
}
}
```
```
import { Component, OnInit } from '@angular/core';
import { MyApiService } from 'ruta-al-servicio';

@Component({
 selector: 'app-mi-componente',
 templateUrl: './mi-componente.component.html',
 styleUrls:['./mi-componente.component.css']
})
export class MiComponenteComponent implements OnInit {
constructor(private myApiService: MyApiService) { }

ngOnInit(): void{
this.myApiService.getData().subscribe({
 next: (data:any) => {
//Manejar la respuesta de la API exitosa (next)
console.log(data);
},
error: (error:any) => {
//Manejar errores
console.error('Error en la solicitud HTTP:', error);
}
});
}
} 
```
**api fake products**  
Fake Api  
[https://github.com/keikaavousi/fake-store-api ](https://fakestoreapi.com/)  
quicktype Convert JSON  
https://quicktype.io/  

**Property Binding**  
Attributes - HTML => Attribute values cannot change  変化しない  
Properties - DOM (Document Object Model) => Property values can change  変化する　　
https://www.youtube.com/watch?v=N8FBmB2jme8  
```
<h2> Welcome {{ name }}}</h2>
<input [id]="myId" type="text" value="Vishwas">
<input id="{{ myId }}" type="text" value="Vishwas">
// properties: myId
//Interpolation（補間：変数や式を埋め込んで、文字列やコードの一部を生成するプロセス）には、Stringでしか使えないという制限がある。
//HTMLプロパティのBooleanをバインドするときはInterpolationが使えない。
<input disabled="false" id="{{ myId }}" type="text" value="Vishwas">
<input disabled={{ false }} id="{{ myId }}" type="text" value="Vishwas">
//この両方の場合、disabledが機能し、falseに意味はない。こういった場合に、プロパティバインディングを使う。  
<input [disabled]="false" id="{{ myId }}" type="text" value="Vishwas">

export class TestComponent implements OnInit {
 public name = "Codevolution";
 public myId = "testId";
 constructor(){}

ngOnInit(){}
}
```
他の方法はクラスを作成する
```
<input [disabled]="isDisabled" id="{{ myId }}" type="text" value="Vishwas">
<input bind-disabled="isDisabled" id="{{ myId }}" type="text" value="Vishwas">

export class TestComponent implements OnInit {
 public name = "Codevolution";
 public myId = "testId";
 public isDisabled = true;
```

**Class Binding**  
https://www.youtube.com/watch?v=Y6OP-lPJxgs  
test.component.ts  
```
<h2 class="text-success">Codevolution</h2>
<h2 [class]="sucessClass">Codevolution</h2>

<h2 class="text-special" [class]="successClass">Codevolution</h2>
// Not italic style. Regular class attribute becomes a dummy attribute
<h2 [class.text-danger]="hasError">Codevolution</h2>
//text-danger will be applied

//To apply multiple classes, [ngClass]
<h2 [ngClass]="messageClasses">Codevolution</h2>
// public hasError = false, public isSpecial = true; => green and italic
// public hasError = true, public isSpecial = true; => red and italic



styles: [
.text-success{
color:green;
}

.text-danger{
color:red;
}

.text-special {
font-style:italic;
}
]
export class TestComponent implements OnInit {
 public name = "Codevolution";
 public successClass = "text-success";
 public hasError = true;
 public isSpecial = true;
 public messageClasses = {
    "text-success": !this.hasError,
    "text-danger": this.hasError,
    "text-special": this.isSpecial
}


 constructor(){ }

 ngOnInit(){
 }
}
```
**Style Binding**  
https://www.youtube.com/watch?v=q256X6-u9Q8  
```
@Component({
 selector: 'app-test',
 template: `
<h2> Welcome {{ name }} </h2>
<h2 [style.color]="'orange'"> Style Binding</h2>
<h2 [style.color]="hasError ? 'red' : 'green'"> Style Binding</h2>
//hasError is true, red. if not, green.
<h2 [style.color]="hightlightColor"> Style Binding</h2>
//hightlightColor: orange
<h2 [ngStyle]="titleStyles">Style Binding</h2>
//blue
`,
styles: []
})
export class TestComponent implements OnInit {

    public name = "Codevolution";
    public hasError = false;
    public isSpecial = true;
    public hightlightColor = "orange";
    public titleStyles = {
        color:"blue",
        fontStyle:"italic"
}
```

**Event Binding**  
https://www.youtube.com/watch?v=ZfIc1_oj7uM  
```
@Component({
 selector: 'app-test',
 template: `
    <h2>Welcome {{ name }}</h2>
    <button (click)="onClick( $event )">Greet</button>
    //"Welcome to Codevolution" and MouseEvent in console

    <button (click)="greeting='Welcome Vishwas'">Greet</button>
    //"Welcome Vishwas" and no console

    {{ greeting }}
`,
styles: []
})
export class TestComponent implements OnInit {

    public name = "Codevolution";
    public greeting="";

    constructor(){ }

    ngOnInit(){
}

    onClick(event){
    console.log('Welcome to Codevolution');
    this.greeting = 'Welcome to Codevolution'
    this.greeting = event.type;
    //"click" and MouseEvent in console

```
**Template Reference Variables**
https://www.youtube.com/watch?v=Oo0-r_YhoJs  
```
@Component({
 selector: 'app-test',
 template: `
    <h2>Welcome {{ name }}</h2>
    <input #myInput type="text">
    <button (click) ="logMessage(myInput.value)">Log</button>
    //What you typed ("Vishwas") in the console

    <button (click) ="logMessage(myInput)">Log</button>
    //<input type="text"> in the console
`,
styles: []
})
export class TestComponent implements OnInit {

    public name = "Codevolution";

    constructor(){ }

    ngOnInit(){
}

    logMessage(value){
    console.log(value);
}

```

**Two way binding**  
https://www.youtube.com/watch?v=DOWwWsbG1Sw  
```
@Component({
 selector: 'app-test',
 template: `
    <input [(ngModel)]="name" type="text">
    {{ name }}
`,
 styles: []
})
export class TestComponent implements OnInit {
    public name="";

    constructor(){ }
    ngOnInit(){
    }

}
```

**ngModel**  
Angularで input 要素の value 属性を設定する場合、通常は ngModel ディレクティブを使います。  
1. Angularフォームモジュールを利用するために FormsModule をインポート  
```
// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent,
  ],
  imports: [
    BrowserModule,
    FormsModule, // FormsModuleをインポート
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
2. コンポーネントのテンプレートで ngModel ディレクティブを使って input 要素の value をバインド  
```
// app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <input type="text" [(ngModel)]="value">
    <p>Value: {{ value }}</p>
  `,
})
export class AppComponent {
  value: string = "initial value";
}

```
Angular 12以降では、FormsModule が ReactiveFormsModule に統合  

**ngIf**  
https://www.youtube.com/watch?v=nWst87nQmZQ
```
// test.component.ts
import { Component, OnInit } from '@angular/core';

@Component({
    selector: 'app-test',
    template:`
     <h2 *ngIf="true">
        Codevolution
    </h2>
    // "Codevolution" appears in the console

    <h2 *ngIf="false">
        Codevolution
    </h2>
    //"Codevolution" doesn't appear in the console
    <h2 *ngIf="displayName">
        Codevolution
    </h2>
    //displayName = true;
    //"Codevolution" appears in the console
    //displayName = false;
    //"Codevolution" doesn't appear in the console

    <h2 *ngIf="displayName; else elseBlock">Codevolution</h2>

    <ng-template #elseBlock>
    <h2>
        Name is hidden
    </h2>
    </ng-template>
    //else block is a reference to this block of HTML
    //ng template tag is like a container for other elements that the ng-if directive can use to properly add or remove blocks of HTML from the DOM
    //    displayName = false;
    //  shows elseBlock "Name is hidden" appears in html

    <div *ngIf="displayName; then thenBlock; else elseBlock"></div>
    <ng-template #thenBlock>
        <h2>Codevolution</h2>
    </ng-template>

    <ng-template #elseBlock>
        <h2>Hidden</h2>
    </ng-template>
    // displayName = false;
    // shows elseBlock "Hidden"
    // displayName = true;
    // shows thenBlock "Codevolution"
 `,
})
export class TestComponent implements OnInit {

    displayName = true;

    constructor(){ }

    ngOnInit(){
    }
}
```
**ngSwitch**  
https://www.youtube.com/watch?v=WiDn2y1Ktws
```
import { Component, OnInit } from '@angular/core';

@Component({
    selector: 'app-test',
    template:`
        <div [ngSwitch]="color">
            <div *ngSwitchCase="'red'">You picked red color</div>
            <div *ngSwitchCase="'blue'">You picked blue color</div>
            <div *ngSwitchCase="'green'">You picked green color</div>
            <div *ngSwitchDefault>Pick again</div>
        </div>
        
 `,
})
export class TestComponent implements OnInit {

    puclic color = "red";

    constructor(){ }

    ngOnInit(){
    }
}
```
**ngFor**  
https://www.youtube.com/watch?v=Du3p6QYGs3A  
```
import { Component, OnInit } from '@angular/core';

@Component({
    selector: 'app-test',
    template:`
        <div *ngFor="let color of colors">
            <h2>{{ color }}</h2>
        // red blue green yellow 
        </div>

        <div *ngFor="let color of colors; index as i">
            <h2>{{i}} {{ color }}</h2>
        // 0 red 1 blue 2 green 3 yellow
        </div>

        
        <div *ngFor="let color of colors; first as f">
            <h2>{{f}} {{ color }}</h2>
        // true red false blue false green false yellow
        </div>

        <div *ngFor="let color of colors; last as l">
            <h2>{{l}} {{ color }}</h2>
        // false red false blue false green true yellow
        </div>

        <div *ngFor="let color of colors; odd as o">
            <h2>{{o}} {{ color }}</h2>
        // false red true blue false green true yellow (Odd numbe)
        </div>

        
        <div *ngFor="let color of colors; even as e">
            <h2>{{e}} {{ color }}</h2>
        // true red false blue true green false yellow (Odd numbe)
        </div>
 `,
})
export class TestComponent implements OnInit {

    puclic color = ["red", "blue", "green", "yellow"];

    constructor(){ }

    ngOnInit(){
    }
}
```
**Component Interaction @Input**  
app.component.ts
```
import { Component, OnInit } from '@angular/core';

@Component({
    selector: 'app-root',
    templateUrl: '-/app.component.html',
    styleUrls: ['./app.component.css]
})
export class AppComponent {
    title = 'app';
    public name = "Vishwas";
}
```
app.component.html  [PARENT]
```
<div style="text-align:center">
  <h1>
    Welcome to {{title}}
  </h1>
</div>
<app-test [parentData]="name"></app-test>
```
test.component.ts  [CHILD}
```
import { Component, OnInit, Input } from '@angular/core';

@Component({
    selector: 'app-root',
    template:`

        <h2>{{"Hello " + parentData}}</h2>
        // "Hello Vishwas" in the browser
`,
    styles: []
})
export class TestComponent implements OnInit {

    @Input() public parentData;

    
    @Input('parentData') public name;
    //When you want to use the different property name than the one parent component uses, you can specify an alias
    //I call this property as "name" within this componen, but input is still the parent data.
    //In this case, <h2>{{"Hello " + name}}</h2>

    constructor(){ }

    ngOnInit(){ }
}
```
 **Component Interaction @Output**  
The way that a child component sends data to the parent component is using events.  
Send "hello codevolution" from the text component to the app component, and display in the app component.  
 app.component.html [PARENT]
```
<div style="text-align:center">
    <h1>
        {{ message }}
    </h1>
</div>
<app-test (childEvent)="message=$event" [parentData]="name"></app-test>
//this $ event variable is going to refer to "hey codevolution"
```
test.component.ts  [CHILD}
```
import { Component, OnInit, Input, Output, EventEmitter } from '@angular/core';

@Component({
    selector: 'app-test',
    template:`

        <h2>{{"Hello " + parentData}}</h2>
        <button (click)="fireEvent()">Send Event</button>
`,
    styles: []
})
export class TestComponent implements OnInit {

    @Input('parentData') public name;

    @Output public childEvent = new EventEmitter();

    constructor(){ }

    ngOnInit(){ }
}

    fireEvent(){
        this.childEvent.emit('Hey Codevolution');
}
}
```
app.component.ts
```
import { Component, OnInit } from '@angular/core';

@Component({
    selector: 'app-root',
    templateUrl: '-/app.component.html',
    styleUrls: ['./app.component.css]
})
export class AppComponent {
    title = 'app';
    public name = "Vishwas";
    pulic message ="";
}
```
*On the latest Angular 6 version, in order to work, you will have to comment out the default import { EventEmitter } from 'events' and include  EventEmitter from '@angular/core'  
**Pipes**  
test.component.ts
```
import { Component, OnInit, Input, Output, EventEmitter } from '@angular/core';

@Component({
    selector: 'app-test',
    template:`

        <h2>{{ name }}</h2>
        <h2>{{ name | lowercase }}</h2>
        // codevolution
        <h2>{{ name | uppercase }}</h2>
        // CODEVOLUTION
        <h2>{{ message | titlecase }}</h2>
        // Welcome To Codevolution

        <h2>{{ name | slice:3 }}</h2>
        // evolution (from index 3)
        <h2>{{ name | slice:3:5}}</h2>
        // ev (index from 3 and 4, 5 is excluded)
        <h2>{{ person | json }}</h2>
        // {"firstName":"John", "lastName":"Doe"}

        <h2>{{5.678 | number:'1.2-3'}}</h2>
        // 5.678
        <h2>{{5.678 | number:'3.4-5'}}</h2>
        // 005.6780
        <h2>{{5.678 | number:'3.1-2'}}</h2>
        // 005.68

        <h2>{{ 0.25 | percent }}</h2>
        // 25%

        <h2>{{ 0.25 | currency }}</h2>
        // $0.25
        <h2>{{ 0.25 | currency: 'GBP' }}</h2>
        // £0.25
        //ISO currency code: https://www.iso.org/iso-4217-currency-codes.html
        <h2>{{ 0.25 | currency: 'GBP': 'code'}}</h2>
        // GBP0.25

        <h2>{{ date }}</h2>
        // Sun Dec 03 2017 21:48:52 GMT+0530 (India Standard Time)
        <h2>{{ date | date:'short' }}</h2>
        //12/3/17, 9:49PM
        <h2>{{ date | date:'shortDate' }}</h2>
        //12/3/17
        <h2>{{ date | date:'shortTime' }}</h2>
        //9:50PM
`,
    styles: []
})
export class TestComponent implements OnInit {

    public name = "Codevolution";
    public message = "Welcome to codevolution";
    public person = {
        "firstName": "John",
        "lastName": "Doe"
}

    public date = new Date();

    constructor(){ }

    ngOnInit(){ }
}

}
```

**service**  
https://www.youtube.com/watch?v=y8lwG8IM82k  
app.component.ts
```
import { Component } from '@angular/core';

@Component({
 selector:'app-root',
 templateUrl:'./app.component.html',
 styleUrls:['./app.component.css']
})
export class AppComponent {
title='Codevolution0;
}
```




https://www.youtube.com/watch?v=soInCF7nbDw&t=11209s

https://codechord.com/2012/01/readme-markdown/

https://docs.github.com/ja/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax  

https://code.visualstudio.com/docs/nodejs/angular-tutorial
