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



https://www.youtube.com/watch?v=soInCF7nbDw&t=11209s

https://codechord.com/2012/01/readme-markdown/

https://docs.github.com/ja/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax
