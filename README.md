# ProyectoInitialG

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 11.0.6.

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

##

