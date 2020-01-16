# angular-8-basic-setup
Basic steps for setup angular 8


## 1. Install Node.js

## 2. npm package manager

## 3. Step 1: Install the Angular CLI
 ```
 npm install -g @angular/cli
 npm install -g @ng-toolkit/init (optional)
```

## 4. Create a Project or workspace and initial application
 ```
 ng new project-name
 choose routing yes/no
 select Css type - css or etc
```
## 5. Run project 
 ```js
 ng serve
 ng serve --host 192.168.27.117 --port 8080
 ng serve --liveReload=true|false 
```
 Reference : [click here](https://angular.io/cli/serve)

## 6. For PWA setup
```js
 ng add @angular/pwa
 ng build --prod
 http-server -p 8080 -c-1 dist/<project-name>
```
## 7. Angular Universal for server side rendering (SSR) for SEO friendly project
```js
ng add @nguniversal/express-engine --clientProject <project-name>
```
## 8. For create component 
```
ng generate component componentName --module=app.module
```
## 9 . For Creating route 
Go to [app-routing.module.ts](#) and add route like below
```
	import { HomeComponent } from './home/home.component';

	const routes: Routes = [
	  {path: '', component: HomeComponent}
	];
```
## 10. For Including Jquery 
```
 npm install --save jquery
 npm install --save @types/jquery
```
 add below code in your component.ts file 
```
 import * as $ from 'jquery';
 declare var $: any; 
 		or
 declare var jQuery: any;
```
```
 npm install jquery -- save
```
## 11. For lagy-load-script 
 - Go to that link [https://stackblitz.com/edit/angular-lazy-load-script](https://stackblitz.com/edit/angular-lazy-load-script)
```
ng g service <name>
```
copy paste LazyLoadScriptService file code
```
import below code in app.module.ts 
import { LazyLoadScriptService } from './lazy-load-script.service';
providers: [LazyLoadScriptService]
import below code in app.component.ts 
import { LazyLoadScriptService } from './lazy-load-script.service';
this.lazyLoadService.loadScript('/assets/jquery.min.js').subscribe(_ => {
    console.log('Jquery is loaded!')
	});
```


## 12 . Call APi using data service
 Include following code in [app.module.ts](#)
 ```
 import { HttpClientModule } from '@angular/common/http';
 imports: [HttpClientModule]
```
```
ng g service data
```
 Include following code in [data.server.ts](#)
```
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders, HttpErrorResponse } from '@angular/common/http';
import { Observable, of } from 'rxjs';
import { map, catchError, tap } from 'rxjs/operators';
import { Router, ActivatedRoute } from '@angular/router';
declare var $: any;

@Injectable({
  providedIn: 'root'
})
export class DataService {
  baseUrl: string = 'https://www.example.com/';
  token: string = '';
  httpOptions: any = {
    headers: new HttpHeaders({
      'Content-Type': 'application/x-www-form-urlencoded',
    })
  };
  constructor(
    private http: HttpClient,
    private myRoute: Router,
  ) { }

  callBaseUrl(method, params): Observable<any> {
    return this.http.post<any>(this.baseUrl + method, $.param(params), this.httpOptions).pipe(
      tap(),
      catchError(this.handleError<any>('postData'))
    );
  }

  private handleError<T>(operation = 'operation', result?: T) {
    return (error: any): Observable<T> => {
      console.log(error);
      return of(result as T);
    };
  }

}
```
 Call api from component 
 ```
 	this.params = {
 		id : '2'
 	};
	this.data.callBaseUrl('<api-route>', this.params).subscribe(res => {
	        console.log('api working');
	});
```
