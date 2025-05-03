# Guia de estilo para el desarrollo en GeneXus
Elaborado por [Jorge Salazar](https://www.linkedin.com/in/jorsalazarcasta%C3%B1eda/)
En base a documento de [Daniel Monza](https://uy.linkedin.com/in/daniel-monza-62515112)


## Objetivos
La presente guía se realizó buscando los siguientes objetivos:

  1. Transmitir las mejores prácticas a la hora de desarrollar en GeneXus.
  1. Estandarizar el código escrito. Ya que hay tantas formas de programar como programadores, se intenta simplificar la lectura del código fuente.
  1. Divulgar buenas prácticas de codificación y las novedades del lenguaje.

## Tabla de Contenidos

  1. [Definición de nombres](#definición-de-nombres)
  1. [Dominios enumerados](#dominios-enumerados)
  1. [Identación y espaciado](#identación-y-espaciado)
  1. [Structured Data Types](#structured-data-types)
  1. [Strings](#strings)
  1. [Comentarios](#comentarios)
  1. [Comandos y funciones](#comandos-y-funciones)
  1. [Parámetros](#parámetros)
  1. [Subrutinas](#subrutinas)
  1. [Buenas prácticas](#buenas-prácticas)
  1. [Recursos](#recursos)
  1. [Empresas que utilizan esta guia](#empresas-que-utilizan-esta-guia)
  1. [Traducciones](#traducciones)
  1. [Licencia](#licencia)
  1. [Modificaciones al documento](#modificaciones-al-documento)

## Definición de nombres

  <a name="naming--descriptive"></a><a name="1.1"></a>
  - [1.1](#naming--descriptive) Se debe ser descriptivo con los nombres.
	> Se intenta que el nombre sea autodescriptivo.

    ```javascript
    // mal
    Proc: CliCre

    // bien
    Proc: ClienteCrear
    ```

  <a name="naming--PascalCase"></a><a name="1.2"></a>
  - [1.2](#naming--PascalCase) Utilizar PascalCase al nombrar objetos, atributos y variables.

    ```javascript
    // mal
    clientecrear

    // bien
    ClienteCrear
    ```

  <a name="naming--leading-underscore"></a><a name="1.3"></a>
  - [1.3](#naming--leading-underscore) No utilizar underscore al inicio o final en ningún tipo de objeto, atributo o variable.
    > Esto puede hacer suponer a un programador proveniente de otros lenguajes que tiene algún significado de privacidad.

    ```javascript
    // mal
    &_CliNom = "John Doe"
    &CliNom_ = "John Doe"
    Proc: _ClienteCrear

    // bien
    &ClienteNombre = "John Doe"
    ```

  <a name="naming-enums"></a><a name="1.4"></a>
  - [1.4](#naming-enums) Nombrar los dominios enumerados sin abreviar, comenzando con la entidad en singular y siguiendo con el calificador enumerado en plural. Los valores enumerados deben especificarse en singular.
	> Se realiza de esta forma para que no colisionen atributos con dominios enumerados.

    ```javascript
    // mal
    DocumentoTipo
    DocumentosTipo
    DocumentosTipos
    DocTipos

    // bien
    DocumentoTipos { Venta, Compra, etc}
    DocumentoModos { Credito, Débito}
    ```

  <a name="naming-procs"></a><a name="1.5"></a>
  - [1.5](#naming-procs) Nombrar procedimientos relacionados mediante Entidad + Atributo(depende el caso) + Complemento + Acción.
	> Esto permite agrupar los objetos de la misma entidad en la selección de objetos entre otros. Algunas acciones típicas son Get, Set, Load (para SDT), Insert, Utpdate, Delete, etc. La diferencia entre Set y Update es que Set refiere a un atributo y Update a una entidad.

    ```javascript
    // mal
    CreCli
    UpsertCliente
    FechaCliente

    // bien
    ClienteUpdate
	ClienteDelete
    ClienteFechaModificadoGet
    ClienteFechaModificadoSet
	DocumentoRecalculo
	```

  <a name="naming-gik"></a><a name="1.6"></a>
  - [1.6](#naming-gik) Utilizar [nomenclatura GIK](http://wiki.genexus.com/commwiki/servlet/wiki?1872,GIK) para nombrar atributos. Se pueden crear atributos sin el límite de los 3 caracteres si el nombre no supera los 20 caracteres y mejora la comprensión.
	> Estandard desde los inicios de GeneXus.

    ```javascript
    // mal
    CreCliFch
    FechaCreadoCliente

    // bien
    CliFchCre

    // mejor
    ClienteCreacionFecha
	```

  <a name="naming-trns"></a><a name="1.7"></a>
  - [1.7](#naming-trns) Las transacciones deben tener el nombre de la entidad en singular.
	> Se define así porque en la comunidad GeneXus está claro que queda mejor a la hora de trabaja por ejmeplo con [Business Component](http://wiki.genexus.com/commwiki/servlet/wiki?5846,Toc%3ABusiness+Component). También es requerimiento de algunos patterns GeneXus para su correcta visualización (ej.: K2BTools).

    ```javascript
    // mal
    Trn:Articulos
    Trn:Clientes

    // bien
    Trn:Cliente
    Trn:Articulo
	```

**[Volver al inicio](#tabla-de-contenidos)**

## Identación y espaciado
  <a name="whitespace-tab"></a><a name="2.1"></a>
  - [2.1](#whitespace-tab) Utilizar tabuladores (tab) en lugar de "espacios". De esta forma, cada uno puede visializar la cantidad de espacioes que prefiera, ya que se configura en GeneXus.
    > La identación ofrece a los desarrolladores una mejor lectura del código fuente. Si tomamos una identación estandard, facilitará al resto entednder el código fuente.

	 ```javascript
    // mal
    If &DocumentoTipo = DocumentoTipos.Venta
    msg( "Venta")
    EndIf

    // mal
    If &DocumentoTipo = DocumentoTipo.Venta
    		msg( "Venta")
    EndIf

    // bien
    If &DocumentoTipo = DocumentoTipo.Venta
        msg("Venta")
    EndIf
    ```

  <a name="whitespace-where"></a><a name="2.2"></a>
  - [2.2](#whitespace-where) Se deben identar las condiciónes y comandos dentro de un For each e indicar transacción.

	 ```javascript
    // mal
    For each
    where DocumentoTipo = DocumentoTipo.Venta
    ...
    EndFor

    // mal
    For each
    defined by ClienteNombre
    ...
    EndFor

    // bien
    For each Documento
        where DocumentoTipo = DocumentoTipo.Venta

        ...
    EndFor
    ```
  <a name="whitespace-newline"></a><a name="2.3"></a>
  - [2.3](#whitespace-newline) Si en un [For each](http://wiki.genexus.com/commwiki/servlet/wiki?24744,For%20Each%20command) se especifican where, defined by ú otros, dejar una línea en blanco antes del código.

	```javascript
    // mal
    For each
       where DocumentoTipo = DocumentoTipo.Venta
       If DocTot > LimCreMto
          ...
       EndIf
    EndFor

    // mal
    For each
       defined by ClienteNombre
       For each Documentos
          ...
       EndFor
    EndFor

    // bien
    For each
       where DocumentoTipo = DocumentoTipos.Venta

       If DocTot > LimCreMto
          ...
       EndIf
    EndFor

    // bien
    For each Cliente
       Defined by ClienteNombre

       For each Documentos
          ...
       EndFor
    EndFor
	```

  <a name="whitespace-parms"></a><a name="2.4"></a>
  - [2.4](#whitespace-parms) Dejar un espacio antes de cada parámetro.

	> Hace a la sentencia más sencilla de leer.

	 ```javascript
    // mal
    parm(in:PaisId,out:&PaisNombre);

    // bien
    parm(in:PaisId, out:&PaisNombre);

    // mal
    &Fecha = ymdtod(2017,01,01)

    // bien
    &Fecha = ymdtod(2017, 01, 01)
    ```

**[Volver al inicio](#tabla-de-contenidos)**

## Dominios enumerados
  <a name="enums-use"></a><a name="3.1"></a>
  - [3.1](#enums-use) Evitar la utilización de textos/números fijos cuando pueden existir multiples valores.
    > Simplificar la lectura y no necesitar recordar el texto específico de cada opción.

    ```javascript
    // mal
    If &HttpResponse = "GET"

    // bien
    // Utilizar el dominio reservado HTTPMethod con los posibles valores ( POST, GET)
    If &HttpResponse = HTTPMethod.Get
    ```

  <a name="enums-use"></a><a name="3.2"></a>
  - [3.2](#enums-datatype) Los dominios enumerados cuyo valor quedará registrado en la base de datos, deberán ser de tipo CHAR.
      > Es para facilitar la lectura de las consultas realizadas directamente a la base de datos por el usuario. Es preferible que se utilice CHAR(10 a 20) para optimizar búsqueda mediante índices pequeños.

	```javascript
	// mal
	MovimientoCuenta.Credito 1
	MovimientoCuenta.Debito  2

	// bien
	MovimientoCuenta.Credito "Credito"
	MovimientoCuenta.Debito  "Debito"
	```

  <a name="enums-default"></a><a name="3.3"></a>
  - [3.3](#enums-default) Evitar definir dominios enumerados con valores "Empty" (0 ó "").
      > Luego, por ejemplo, si los queremos desplegar en un combo, no va a funcionar "Empty item".

	```javascript
	// mal
	ModoLectura.Normal     ""
	ModoLectura.Secuencial "S"

	// bien
	ModoLectura.Normal     "Normal"
	ModoLectura.Secuencial "Secuencial"
	```

**[Volver al inicio](#tabla-de-contenidos)**

## Structured Data Types
  <a name="sdt-use"></a><a name="4.1"></a>
  - [4.1](#sdt-use) Utilizar New() en la creación de SDT en lugar de Clone(). Incluso antes de utilizar el SDT por primera vez en lugar de al final (aunque GeneXus lo soporte).
    > Queda claro que se está trabajando con un nuevo item.

    ```javascript
    // &Cliente SDT:Cliente
    // &Clientes lista de SDT:Cliente

    // mal
    For each Cliente
       &Cliente.ClienteNombre = ClienteNombre
       &Clientes.Add(&Cliente.Clone())
    EndFor

    // bien
    For each Cliente
       &Cliente = new()
       &Cliente.ClienteNombre = ClienteNombre
       &Clientes.Add(&Cliente)
    EndFor
    ```
  <a name="sdt-list"></a><a name="4.1"></a>
  - [4.1](#sdt-list) Desde que GeneXus permite definir variables como listas, evitar crear SDT del tipo lista.
    > Al definir la variable del item particular, se lo marca como lista.

    ```javascript
    // mal
    SDT:Clientes : Lista
    	ClienteItem
        	ClienteNombre

    // bien
    SDT:Cliente
       	ClienteNombre
    ```

**[Volver al inicio](#tabla-de-contenidos)**

## Strings

  <a name="strings-format"></a><a name="5.1"></a>
  - [5.1](#strings-format) Utilizar [format](http://wiki.genexus.com/commwiki/servlet/wiki?8406,Format%20function) para desplegar mensajes conteniendo datos y en llamadas a funciones javascript.
    > Si la aplicación se va a traducir en diferentes lenguajes no hay que re-programar los mensajes.

    ```javascript
    // mal
    &Msg = "El cliente Nro." + &ClienteId.ToString() + " se llama " + &ClienteNombre

    // bien
    &Msg = format( "El cliente Nro. %1 se llama %2", &ClienteId.ToString(), &ClienteNombre)
    ```

	Esto soluciona la traducción según contexto. Por ejemplo,

	```
	ingles: "The name of John's dog is Gandalf"

	español: "El nombre del perro de John es Gandalf"
	```

	Lo anterior realizado mediante concatenación no quedaría correctamnete traducido.

	En el siguiente caso, podemos ver como podemos dejar para traducir solo el texto dentro de un método jsevent.

	```javascript
	// mal
	&Msg = "confirm('¿Está seguro de agregar excepción?')"
    &LstExc.JSEvent("onclick", &Msg)

	// bien
	&Msg = format(!"confirm('%1')", "¿Está seguro de agregar excepción?")
    &LstExc.JSEvent("onclick", &Msg)

	```

  <a name="strings-trans"></a><a name="5.2"></a>
  - [5.2](#strings-trans) Utilizar !"" para strings que no deben ser traducidos.
    > Un traductor puede modificar constantes o códigos específicos del sistema y pueden afectar el funcionamiento, por ejemplo parámetros.

    ```javascript
    // mal
    &ParametroValor = ParamGet("GLOBAL ENCRYPT KEY")

    // bien
    &ParametroValor = ParamGet(!"GLOBAL ENCRYPT KEY")
    ```
  <a name="strings-quotation"></a><a name="5.3"></a>
  - [5.3](#strings-trans) Utilizar comilla simple por defecto.
    > Esto lo realizamos así ya que cada evento o subrutina creado por GeneXus, utiliza comilla simple.

	 ```javascript
    // mal
    &Msg = "Hola mundo!"

    // bien
    &Msg = 'Hola mundo!'
    ```

**[Volver al inicio](#tabla-de-contenidos)**

## Comentarios

  <a name="comments--multiline"></a><a name="6.1"></a>
  - [6.1](#comments--multiline) Utilizar `/** ... */` para comentarios multi-línea en descripciones de funcionamiento. Se puede seguir utilizando `//` ya que Genexus permite auto-comentar con Ctrl-Q | Ctrl-Shift-Q.

    ```javascript
    // mal
    // CrearCliente crea una nuevo cliente
    // según las variables:
    // &ClienteNombre
    // &ClienteDirección
    Sub 'CrearCliente'
      // ...
    Endsub

    // bien
    /**
     * CrearCliente crea una nuevo cliente
     * según las variables:
     * &ClienteNombre
     * &ClienteDirección
     */
    Sub 'Cliente crear'
       // ...
    EndSub
    ```

  <a name="comments--singleline"></a><a name="6.2"></a>
  - [6.2](#comments--singleline) Utilizar `//` para comentarios de una sola línea. Estos comentarios deben estar una línea antes del sujeto a comentar. Dejar una línea en blanco antes del comentarios a no ser que sea la pimer línea del bloque o se esté comentando un where de for-each.

    ```javascript
    // mal
    &ClienteNombre = "John Doe" // Se asigna el nombre a la variable

    // bien
    // Se asigna el nombre a la variable
    &ClienteNombre = "John Doe"

    // mal
    Sub 'Cliente crear'
       msg( "Creando cliente", status)
       // Se crea el cliente
       &ClienteBC = new()
       &ClienteBC.ClienteNombre = "John Doe"
       &ClienteBC.Save()
    EndSub

    // bien
    Sub 'Cliente crear'
       msg( "Creando cliente", status)

       // Se crea el cliente
       &ClienteBC = new()
       &ClienteBC.ClienteNombre = "John Doe"
       &ClienteBC.Save()
    EndSub

    // también está bien
    Sub 'Cliente crear'
       // Se crea el cliente
       &ClienteBC = new()
       &ClienteBC.ClienteNombre = "John Doe"
       &ClienteBC.Save()
    EndSub
    ```
  <a name="comments--spaces"></a><a name="6.3"></a>
  - [6.3](#comments--spaces) Comenzar todos los comentarios con un espacio para que sean sencillos de leer.

    ```javascript
    // mal
    //Está activo
    &IsActive = true

    // bien
    // Está activo
    &IsActive = true

    // mal
    /**
     *Se obtiene el nombre de la empresa
     *para luego desplegarlo
     */
    &EmpresaNombre = EmpresaNombreGet(&EmpresaId)

    // bien
    /**
     * Se obtiene el nombre de la empresa
     * para luego desplegarlo
     */
    &EmpresaNombre = EmpresaNombreGet(&EmpresaId)
    ```

  <a name="comments--actionitems"></a><a name="6.4"></a>
  - [6.4](#comments--actionitems) Agregar pefijos en los comentarios con `FIXME` o `TODO` ayudan a otros desarrolladores a entender rapidamente si se está ante un posible problema que necesita ser revisado o si se está sugiriendo una solución a un problema existente. Estos son diferentes a los comentarios regulares porque conllevan a acciones. Estas acciones son `FIXME: -- necesita resolverse` or `TODO: -- necesita implementarse`.

  <a name="comments--fixme"></a><a name="6.5"></a>
  - [6.5](#comments--fixme) Usar `// FIXME:` para marcar problemas.

    ```javascript
    // FIXME: Revisar cuando &Divisor es 0
    &Total = &Dividendo / &Divisor
    ```

  <a name="comments--todo"></a><a name="6.6"></a>
  - [6.6](#comments--todo) Usar `// TODO:` para marcar implementaciones a realizar.

    ```javascript
    // TODO: Implementar la subrutina
    Sub "Cliente crear"
    EndSub
    ```

**[Volver al inicio](#tabla-de-contenidos)**

## Comandos y funciones

  <a name="commands--naming"></a><a name="7.1"></a>
  - [7.1](#commands--naming) Utilizar minúsculas al nombrar comandos y funciones del sistema.
	> Esto optimiza el desarrollo ya que los comandos y funciones provistas por el lenguaje se utilizan tan frecuentemente y no es necesario especificarlos en PascalCase.

    ```javascript
    // mal
    For each Cliente
       Where ClienteCodigo = &ClienteCodigo
       Msg(ClienteNombre)
    EndFor

    // bien
    For each Cliente
       where ClienteCodigo = &ClienteCodigo

       msg(ClienteNombre)
    EndFor

    // mal
    &Fecha = YmdToD(2017, 01, 01)

    // bien
    &Fecha = ymdtod(2017, 01, 01)
    ```

  <a name="commands--case"></a><a name="7.2"></a>
  - [7.2](#commands--case) Utilizar [do case](http://wiki.genexus.com/commwiki/servlet/wiki?31605,Do%20Case%20command) siempre que se pueda a fín de sustituir [if](http://wiki.genexus.com/commwiki/servlet/wiki?8608,If+Command,) anidados. Dejar un espacio entre cada bloque de case.

    ```javascript
    // mal
    If &DocTipo = DocumentoTipos.Venta
       ...
    else
   	   If &DocTipo = DocumentoTipos.Compra
          ...
       EndIf
	EndIf

    // también mal
    Do case
       case &DocTipo = DocumentoTipos.Venta
          ...
       case &DocTipo = DocumentoTipos.Compra
          ...

	EndCase

    // bien
    Do case
       case &DocTipo = DocumentoTipos.Venta
          ...

       case &DocTipo = DocumentoTipos.Compra
          ...

       otherwise
          ...
	EndCase

    // también está bien - Cuando existen multiples case y la acción es de una sola línea.
	> Esto facilita leer todas las opciones sin necesidad de scroll
    Do case
       case &Action = Action.Update      do 'DoUpdate'
       case &Action = Action.Insert      do 'DoInsert'
       case &Action = Action.Regenerate  do 'DoRegenerate'
       case &Action = Action.Clean       do 'DoClean'
       case &Action = Action.Refresh     do 'DoRefresh'
       case &Action = Action.Reload      do 'DoReload'
       otherwise	do 'UnexpectedAction'
	EndCase
    ```

  <a name="commands--foreach-where"></a><a name="7.3"></a>
  - [7.3](#commands--foreach-where) Utilizar clausula where en comandos [For each](http://wiki.genexus.com/commwiki/servlet/wiki?24744,For%20Each%20command) en lugar de usar comandos "if", siempre que se trate de atributos de la [tabla extendida](http://training.genexus.com/resumen-de-conceptos-fundamentales-de-genexus-es#tabla-base-y-tabla-extendida-resumen-de-conceptos-fundamentales).
	> Con esto logramos trasladar la condición al DBMS y hacer que forme parte de la query select evitando trabajar con grandes volumenes de datos en el servidor de aplicación ó eventualmente en el cliente.

    ```javascript
    // mal
    For each Documento
       If DocTipo = DocumentoTipos.Ventas
          ...
       EndIf
    EndFor

    // bien
    For each Documento
       where DocTipo = DocumentoTipos.Ventas
       ...
    EndFor
    ```

  <a name="commands--foreach-when"></a><a name="7.4"></a>
  - [7.4](#commands--foreach-when) Utilizar "when" en comandos [For each](http://wiki.genexus.com/commwiki/servlet/wiki?24744,For%20Each%20command) para simplificar la query enviada al DBMS.

    ```javascript
    // mal
    For each Documento
       where DocumentoTipo = DocumentoTipos.Ventas
       where DocumentoFecha >= &DocumentoFechaInicio or null(&DocumentoFechaInicio)
       ...
    EndFor

    // bien
    For each Documento
       where DocumentoTipo = DocumentoTipos.Ventas
       where DocumentoFechaInicio >= &DocumentoFechaInicio when not &DocumentoFechaInicio.IsEmpty()

       ...
    EndFor
    ```

  <a name="commands--foreach-when"></a><a name="7.5"></a>
  - [7.4](#commands--syntax) Utilizar la última sintaxis siempre que la versión lo soporte.

    ```javascript
    // mal
	&Name = udp(NameGet, &Id)
	
	// bien
	&Name = NameGet(&Id)
	
	// mal
    call(NameSet, &Id, &Name)
	
	// bien
    PNameSet(&Id, &Name)
	
	// mal
	&Num = val(&NumChar)
	
	// bien
	&Num = &NumChar.ToNumeric()
    ```

**[Volver al inicio](#tabla-de-contenidos)**

## Parámetros

  <a name="parms--sdt"></a><a name="8.1"></a>
  - [8.1](#parms--sdt) Utilizar SDT en lugar de multiples parámetros.
	> Esto es importante en los casos de objetos con varios parámtros in o out, ya que la lectura queda confusa y si hay que modificar parámetros, se deberá revisar todos los llamadores. Si hay varios parámetros de salida, se debrán crear SDT por separado, tanto para entrada como para salida.

    ```javascript
	 // mal
	 parm(in:&CliNom, in:&CliApe, in:&CliTel, in:&CliDir, in:&CliDOB);

	 // bien
	 parm(in:&sdtCliente);

	 // Ejemplo de un webservice
	 // mal
	 parm(in:&Nombre, in:&Edad, in:&EstadoCivil, out:&Id, out:&ErrorId);

	 // bien
	 parm(in:&PersonCreateRequest, out:&PersonCreateResponse);
    ```

**[Volver al inicio](#tabla-de-contenidos)**

## Buenas prácticas

  <a name="bpractices--ver"></a><a name="10.1"></a>
  - [10.1](#bpractices--ver) Versionar el sistema según xx.yy.zz.

  Donde:
	- xx: Cambios mayor de versión del sistema. Generalmente implica un cambio mayor en el sistema, por ejemplo un nuevo modulo funcional.
	- yy: Incorpora cambios en base de datos.
	- zz: Incorpora solo cambios en los binarios o de diseño

  <a name="bpractices--ver"></a><a name="10.2"></a>
  - [10.2](#bpractices--ver) Disponer de la versión actual de la aplicación dentro de los binarios.
	> Esto permite de forma inequivoca saber en que versión de la aplicación estamos trabajando. La versión se puede guardar también como un parámtetro dentro de la base de datos, para poder obtener la diferencia con la versión de los binarios y así realizar la acción deseada.

	Para lograr esto, se crea un procedimiento que retorna la versión en que estamos trabajando:

	```javascript
	// Parameters
	parm(out:&Version)

	// Source
	&Version = !"1.5.6"
	```

  <a name="bpractices--defpro"></a><a name="10.3"></a>
  - [10.3](#bpractices--defpro) Propiedades por defecto

  Isolation level: Read commited  
  Generate prompt programs: No

  <a name="bpractices--pass"></a><a name="10.4"></a>
  - [10.4](#bpractices--pass) No mostrar contraseñas en logs e información de debug
	> Esto obedece a mejorar la seguridad de los sistemas, evitando que queden credenciales en archivos y consolas con sus potenciales riesgos de seguridad

  <a name="bpractices--sdt"></a><a name="10.5"></a>
  - [10.5](#bpractices--sdt) Establecer namespaces específicos en SDTs utilizados en webservices
	> Esto evita que se generen inconvenientes en producción si el environment cambia de namespace por defecto. Esto se define en la propiedad "name space" del SDT.

  <a name="bpractices--grids"></a><a name="10.6"></a>
  - [10.6](#bpractices--grids) Evitar cargar grillas por defecto
	> En la mayoría de los casos el usuario va a aplicar algún filtro y al cargar por defecto se desperdician recursos del DBMS.

<a name="bpractices--null"></a><a name="10.7"></a>
  - [10.7](#bpractices--null) Evaluar si crear atributos nuevos como "null"
	> Ayuda a no re-crear la tabla ante una Reorg. Especialmente en tablas grandes donde el tiempo de migración de datos puede ser demasiado largo. 

<a name="bpractices--session"></a><a name="10.8"></a>
  - [10.8](#bpractices--null) Evitar acceder a sesiones (websession) desde procedimientos con lógica de negocios.
	> El acceso a sesiones debe ser responsabilidad de la interfaz. Al trabajar con sesiones dentro de procedimietos estamos introduciendo lógica de la interfaz en el dominio del problema. Debido a esto, depues podemos tener problemas si deseamos utilizar dichos procedimietnos en ejecuciones por consola batch o win.

<a name="bpractices--business"></a><a name="10.9"></a>
  - [10.9](#bpractices--business) No mantener lógica del negocio en la interfaz.
	> Siguiendo con la idea anterior, se debe evitar incoporar lógica de negocios en la interfaz.
  El caso más claro en web es generar la exportación a excel y reportes en webpanels. Si en lugar de ello, los encapsulamos en procedimientos, eventualmente los podemos generar desde otras interfaces.

## Recursos

  - [GeneXus Wiki](http://wiki.genexus.com/) - GeneXus
  - [GeneXus Training](http://training.genexus.com) - GeneXus
  - [GeneXus Developpers](http://developers.genexus.com) - GeneXus
  - [GeneXus Marketplace](http://marketplace.genexus.com) - GeneXus
  - [GeneXus Search](http://search.genexus.com/) - GeneXus
  - [Stackoverflow](https://es.stackoverflow.com/questions/tagged/genexus)

## Empresas que utilizan esta guia

  Esta es una lista de las empresas que están utilizando esta guia de desarrollo. 

- [**Sincrum**](http://sincrum.com)
- [**Valkimia**](https://valkimia.com/)
- [**Tangocode**](http://tangocode.com)
- [**GeneXus**](https://www.genexus.com)
- [**TributApp**](https://www.tributapp.com)
- [**I+Dev**](http://www.imasdev.com)
- [**Big Cheese**](https://bigcheese.com.uy)
- [**Neuronic**](https://neuronic.com.ar/)



**[Volver al inicio](#tabla-de-contenidos)**

## Traducciones
Esta guia de estilo se encuentra también en los siguientes lenguajes:

  - ![us](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/United-States.png) [**English**](README_en.md) by [Laura Aguiar](https://uy.linkedin.com/in/laura-aguiar-396aa56)

Basado en [la guia de Javascript de AirBNB](http://airbnb.io/javascript/)

**[Volver al inicio](#tabla-de-contenidos)**

## Modificaciones al documento

Recomendamos que ralice un fork de está guía, realice modificaciones y/o cambie las reglas para que se adecuén a su equipo de trabajo ó empresa. A continuación puede agregar modificaciones a la guía de estilos. Esto le permite actualizar periódicamente el docuemnto sin lidiar con problemas de merge.
