# Ferretería Lozada (v1)

Primera versión académica del sitio web de **Ferretería Lozada**, ferretería del sur de Quito. Este repositorio se llamaba `FerreteriaLozada` y ahora se llama `ferreteria-lozada-v1`. Conserva el entregable del proyecto final de Desarrollo Web (USFQ): una página de presentación, un catálogo con carrito de cotización por WhatsApp y un panel para administrar los productos.

La versión que sigue en uso está en [tienda-ferreteria-lozada](https://github.com/Scorpwik/tienda-ferreteria-lozada).

El sitio no cobra en línea ni pide datos bancarios. El cliente arma la lista y la envía por WhatsApp; el local confirma stock, pago y entrega en esa conversación.

## Características

### Página de inicio

`index.html` presenta el local:

- Saludo de portada, acceso al catálogo y botón de WhatsApp.
- Historia del negocio, horario (8:30 a 17:00, lunes a viernes) y mapa de Google Maps con enlace para abrir la ubicación.
- Tres pasos para hacer el pedido: armar la lista, enviarla por WhatsApp y coordinar la entrega.

### Catálogo y cotización

`catalogo.html` y `js/app.js` leen la colección `productos` de Cloud Firestore en tiempo real.

- Búsqueda por nombre o descripción.
- Filtros por categoría, disponibilidad (en stock o agotado) y rango de precio.
- Ocho categorías principales, más «Otros» cuando el texto no encaja: Plomería y Tuberías, Eléctricos e Iluminación, Herramientas Manuales, Herramientas Eléctricas y Maquinaria, Construcción y Materiales, Ferretería y Fijaciones, Pinturas y Químicos, y Seguridad y Protección.
- Los documentos con el mismo nombre se muestran como un solo producto con selector de medida.
- Precio por unidad y, si el producto lo tiene, precio por ciento.
- Cantidad con botones de más y menos. Si está agotado, no se puede agregar.
- Vista de detalle del producto.
- El listado carga de 30 en 30, con el botón «Mostrar mas productos».
- Carrito lateral de cotización, con contador en la barra y botón flotante. El carrito vive en la memoria del navegador: no se guarda al cerrar la página.
- «Enviar Cotización» abre un enlace `wa.me` con el detalle, los subtotales y el total referencial. El número es el que ya está en la portada y en `js/app.js`.

### Panel de administración

`admin.html` y `js/admin.js` piden correo y contraseña con Firebase Authentication. Sin sesión, el panel permanece oculto. En las páginas públicas no hay un enlace visible: `js/security.js` abre `admin.html` con Ctrl + Shift + A, o con tres clics seguidos en el logo.

Con la sesión iniciada se puede:

- Crear, editar y eliminar productos.
- Marcar un producto como en stock o agotado.
- Cambiar el precio por unidad y el precio por ciento desde la tabla.
- Subir una imagen (el navegador la comprime antes de guardarla) o pegar una URL. También se puede quitar la imagen y buscar una referencia en Google Imágenes.
- Crear otra medida del mismo producto y agrupar un producto con otro que ya existe.
- Definir secciones de medidas y sus opciones. Esas listas se guardan en la colección `medidas`.
- Buscar y filtrar la tabla por nombre, categoría, stock y precio.
- Normalizar en Firestore la categoría de los productos hacia las ocho categorías principales.

## Tecnologías

No hay instalador ni paso de compilación. El navegador carga todo desde el HTML.

| Pieza | Uso en este proyecto |
| --- | --- |
| HTML5 | `index.html`, `catalogo.html` y `admin.html` |
| CSS3 | `css/styles.css` y `css/admin.css`. Naranja `#f54500`, negro y blanco |
| [Bootstrap 5.3.2](https://getbootstrap.com/) y Bootstrap Icons 1.11.4 | Grilla, barra de navegación, modales, carrito lateral y botones. El catálogo se acomoda en celular y en computadora |
| JavaScript (módulos ES) | `js/app.js`, `js/admin.js` y `js/security.js` |
| [Firebase JS 10.12.0](https://firebase.google.com/) | Cloud Firestore para productos y medidas. Authentication para el panel. No se usa Firebase Storage: la foto queda en el documento como imagen comprimida o como URL |
| Google Fonts | Poppins en títulos y Inter en el texto |
| WhatsApp | Enlaces `wa.me`, no una pasarela de pago |
| Google Maps | Mapa embebido en la portada |

`Validacion_y_test/` es una presentación aparte, con los resultados de las pruebas de usabilidad y de diseño. No forma parte de la tienda.

## Cómo ejecutarlo

Hace falta un navegador actual y conexión a internet: Firebase, Bootstrap y las fuentes se cargan desde sus CDN, y el catálogo sale de Firestore. La configuración del proyecto de Firebase ya está en `js/app.js` y `js/admin.js`.

Los scripts son módulos, así que abrir el HTML directo con `file://` no alcanza. Desde la carpeta del repositorio, levante un servidor estático:

```bash
python3 -m http.server 8080
```

Luego abra [http://localhost:8080](http://localhost:8080). El catálogo está en [http://localhost:8080/catalogo.html](http://localhost:8080/catalogo.html) y el panel en [http://localhost:8080/admin.html](http://localhost:8080/admin.html).

Para entrar al panel se usa una cuenta de Firebase Authentication de ese proyecto. Este repositorio no guarda el correo ni la contraseña. «Salir» cierra la sesión.

## Autor

**Joshua Lozada** ([Scorpwik](https://github.com/Scorpwik)), autor y dueño del repositorio.

El entregable académico también lo firmaron Juan Diego Cadena y Raymond Portilla, en el proyecto final de Desarrollo Web de la Universidad San Francisco de Quito.
