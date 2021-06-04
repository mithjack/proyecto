# Proyecto de expansión de InitialG, Modulo Tienda

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 11.0.6.

Vamos a explicar los componentes que componen el codigo de la tienda, para poder entender el codigo.

El modulo se encuentra en /src/app/tienda

## Tienda

Por defecto la pagina inicial de la tienda es el componente inicio-tienda.

HTML:

-> El codigo está comentado para su mejor entendimiento.

-> El html se estructura por un apartado comun contenido por la sección que engloba toda la página, hasta el menú categorias, este menú nos cambiará el template del componente a elementos filtrados o todos los elementos.
Para las categorias utilizaremos un ngFor para recorrer las categorias que tenemos en la base de datos (utilizamos en inicio-tienda.component.ts el modelo categoria para darle valor y usaremos la función getCategorias() para suscribirnos a las categorias. Esta función la encontraremos en el fichero de tienda.service.ts ya que otros componentes lo llamaran)

-> Mediante un ngIf elegiremos el contenido de todo o el filtrado; todo el contenido estará almacenado en un grid, para que sea responsive se le ha dado tamaño en función del tamaño de la pantalla "col-11 col-sm-5 col-md-3 col-lg-3 col-xl-2" y en caso de que sean los productos filtrados, un *ngIf="producto.disponible&&producto.cat_id==filtro.clase" se encarga de que solo aparezcan los correctos.

En el component:

->Importaremos los datos de la API pasandolos por los modelos de Catalogo y Categorias; para poder recibir los paremetros por la ruta, importaremos tambien import { ActivatedRoute, NavigationEnd, Params, Router } from '@angular/router'; como el apartado logico lo realizamos en el servicio externo, deberemos importarlo tambien import { TiendaService } from '../services/tienda.service'; (cada vez que vayamos a usarlo, despues de declararlo en el constructor, deberemos llamarlo como this.{nombre del constructor}.{funcion a llamar}) y finalmente, ya que algunos componentes se iran actualziando segun cambien los valores, importaremos el datashare import { DataSharingService } from '../../data-sharing.service';

->Primero declararemos las variables:  "catalogo: Catalogo;" y  "categoria: Categorias;"  Estas seran el modelo que usaremos para los datos de la api, filtro: {clase: string}; este será los datos que filtraremos para los resultados de la tienda.

-> Para poder utilizar los imports en el fichero, deberemos llamarlos en el constructor (este paso lo realizaremos siempre que importemos algun componente en cualquiera de los ficheros que realizemos)

private servicioTienda: TiendaService -> Este será el que realice las llamadas al servicio externo.

private rutaActiva: ActivatedRoute, -> Este extraerá los datos que le mandemos por parametro.

private router:Router, -> Este nos permitirá navegar a otras paginas desde dentro del componente.

private dataSharingService: DataSharingService -> Nos servirá para realizar los BehaviorSubject.

->El contenido pues es OnInit, cargaremos todos los datos (en las páginas que importemos datos de arranque utilizaremos esta funcion) que nos cargara los productos, las categorias y el filtro de la tienda (en caso de haberlo mandado) y se suscribirá a estos;  luego llamaremos al servicio getUserLog, el cual nos dará la información del usuario en caso de que este haya iniciado sesión.

## Vista-producto 

HTML:

Nada más abrir el primer div, creamos un ngFor que recorra el catalogo y un ngIf que nos filtre solo al producto seleccionado de la tienda.

Para evitar que se nos puedan colar en algun articulo que aun no hayamos puesto a la venta, dentro del main crearemos *ngIf="producto.disponible; else no" con esto, si el articulo seleccionado NO está disponible, pasaremos al ng-template #no que nos mostrará un error de que no existe el producto elegido y nos dará un boton para volver a la tienda.

La vista será dentro de un grid, a la izquierda insertaremos la imagen y a la derecha introduciremos los datos.
Si el producto tiene algun estado le añadiremos una etiqueta encima en función del estado (si es 1 que seria nuevo, insertaremos un pequero simbolito que ponga NEW).

Para mostrar los precios con IVA, hemos creado esta función {{producto.precio--((producto.precio *producto.IVA)/100) | number: '1.0-2'}} que nos calculará el iva y el pipe nos mostrará el resultado como un numero con 0 a 2 decimales.

Si el stock del articulo no es 0, crearemos un [(ngModel)]="cantidad_articulos" que nos permitirá pasar el valor que introduzcamos al component para poder realizar las gestiones que hagan falta. Gracias a este ngModel, podemos desactivar el boton de enviar si no cumple los requisitos minimos [disabled]="cantidad_articulos<1||cantidad_articulos>producto.stock" (que no haya stock o que los articulos sean más que los que hay en stock).


En el component:

-> Al igual que en otros ficheros component, importaremos los modelos que usaremos en la gestion de datos de la API, el fichero de servicios, la ruta activa para recibir datos y el datasarhe para e behavior subject.

-> Inicialmente como en la mayoria, llamaremos a cargarTodo() en el OnInit, para que nos cargue los datos de la página. Luego asignaremos 1 al valor de cantidad_articulos (cambiará si le asignamos otra cantidad en la vista del producto) y la función par cargar el usuario local si estaba ya logueado.

-> Cuando añadamos algun producto al carrito, llamaremos a addToCart(producto) donde le pasaremos al servicio con el mismo nombre el producto y la cantidad de articulos; el servicio buscará en el carro si el articulo está dentro, añadiendolo en el caso de no encontrarlo o modificandolo si ya se encontraba dentro.
Luego guardará el carro en un BS y tambien almacenaremos localmente el carro. 
Una función se encargará de recalcular el precio total del contenido del carro y actualizar el BS de este.

-> goBack simplemente nos hace un router de nuevo a la Tienda.

##
