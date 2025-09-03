# Diseño software

>!-- ## Notas para el desarrollo de este documento
>En este fichero debeis documentar el diseño software de la práctica.

 >:warning: El diseño en un elemento "vivo". No olvideis actualizarlo
 >a medida que cambia durante la realización de la práctica.

 >:warning: Recordad que el diseño debe separar _vista_ y
 >_estado/modelo_.
	 

>El lenguaje de modelado es UML y debeis usar Mermaid para incluir los
diagramas dentro de este documento. Por ejemplo:

```mermaid
classDiagram
    class DrinkObject {
	- _name: str
	- _id: str
	+ name(): str
	+ id(): str
	+ __init__(name: str)
	+ __repr__(): str
	}
    class ViewHandler {
	+ on_search_clicked(name: str): void
	+ on_category_changed(category: str): void
	+ on_ingredient_changed (ingredient: str): void
	}

    class Model {
	- categories: list
	- ingredients: list
	+ check_internet_connection(self): void
	+ get_categories(): list
	+ get_drinks_by_category(category: str): list
  	+ do_search_name(name: str): list
  	+ get_drink_details(id: str): dict
	+ get_ingredient_details(name: str): dict
	+ get_ingredients(): void
	+ get_drinks_by_ingredient(ingredient): void
	+ get_image_url(id: str): str
	+ get_image(url): void

	}
    class View {
	- handler: ViewHandler
  	- selected_drink: None
  	- drink_name_to_id: dict
  	- drink_list: List
  	- drink_image: Gtk.Image
	- ingredient_names: dict
	- ingredient_measure_list: list
  	- category_combobox: Gtk.ComboBoxText
	- ingredient_combobox = Gtk.ComboBoxText()
	- loading_dialog = None

  	+ set_handler(handler: ViewHandler): void
  	+ on_activate(app: Gtk.Application): void
  	+ crearInterfaz(app: Gtk.Application): void
	+ on_search_bar_changed(widget): void
  	+ update_drink_list(data: list): void
	+ update_ingredient_list(ingr_me, ingredients) : void
  	+ update_categories(categories: list): void
	+ update_ingredients (ingredients) : void
  	+ on_drink_selected(widget: Gtk.Widget, row: Gtk.ListBoxRow): void
	+ on_ingredient_selected (widget, row) : void
  	+ on_category_changed(widget: Gtk.ComboBoxText): void
	+ on_ingredient_changed (widget) : void
  	+ update_image(image_url: str): void
	+ show_error_message(text : str): void
	+ on_error_dialog_response(dialog): void
	+ show_loading_dialog(self): void
	+ hide_loading_dialog(self): void
	}

   class Presenter {
	- model: Model
	- view: View
	+ run(application_id: str): void
	+ load_comboboxes(self): void
	+ on_search_clicked(name: str): void
	+ search_name(name: str): void
	+ waiting(self): void
	+ update_drink(result: str): void
	+ work_drink_details(self, drink_id: str, language) : void
	+ get_drink_details(drink_id: str): dict
	+ get_ingredient_details(ingredient_name: str) : dict
	+ get_image(url): void
	+ on_category_changed(category: str): void
	+ on_ingredient_changed(ingredient) : void
	+ search_by_category(category): void
	+ search_by_ingredient(ingredient) void
	+ update_image(drink_id: str): void
	}
  class ServerError{
        }
  class NotFoundError{
        }
    


	DrinkObject --> View
	ViewHandler --> View
	Presenter --> Model
	Presenter --> View
	NotFoundError --> Model
	ServerError --> Model
	
	View ..> Gtk : << uses >>
	Presenter ..> locale : << uses >>
	Presenter ..> threading : << uses >>

	<<package>> Gtk

	<<package>> locale

	<<package>> threading

```
>Diagrama de secuencia que contiene los casos de uso de listar categorías, listar ingredientes y buscar cócteles por categoría
```mermaid
sequenceDiagram
    participant View
    participant Presenter
    participant Model
    participant Main
    actor User

    Main->>Presenter: Iniciar la aplicación
    Presenter->>Model: Obtener categorías
    Model->>Model: Realizar solicitud HTTP para obtener categorías
    Model-->>Presenter: Categorías
    Presenter->>Model: Obtener ingredientes
    Model->>Model: Realizar solicitud HTTP para obtener ingredientes
    Model-->>Presenter: Ingredientes
    Presenter->>View: Actualizar la vista con categorías e ingredientes
    Presenter->>View: Ejecutar la vista
    User->>View: Seleccionar categoría
    View->>Presenter: Solicitar nombres de cócteles por categoría
    Presenter->>Model: Obtener nombres de cócteles por categoría
    Model->>Model: Realizar solicitud HTTP para obtener cócteles por categoría
    Model-->>Presenter: Cócteles por categoría
    Presenter->>View: Mostrar cócteles por categoría
```
>Diagrama de secuencia que contiene los casos de uso de listar categorías, listar ingredientes y buscar cócteles por ingrediente
```mermaid
sequenceDiagram
    participant View
    participant Presenter
    participant Model
    participant Main
    actor User

    Main->>Presenter: Iniciar la aplicación
    Presenter->>Model: Obtener categorías
    Model->>Model: Realizar solicitud HTTP para obtener categorías
    Model-->>Presenter: Categorías
    Presenter->>Model: Obtener ingredientes
    Model->>Model: Realizar solicitud HTTP para obtener ingredientes
    Model-->>Presenter: Ingredientes
    Presenter->>View: Actualizar la vista con categorías e ingredientes
    Presenter->>View: Ejecutar la vista
    User->>View: Seleccionar ingrediente
    View->>Presenter: Solicitar nombres de cócteles por ingrediente
    Presenter->>Model: Obtener nombres de cócteles por ingrediente
    Model->>Model: Realizar solicitud HTTP para obtener cócteles por ingrediente
    Model-->>Presenter: Cócteles por ingrediente
    Presenter->>View: Mostrar cócteles por ingrediente
```
>Diagrama de secuencia que contiene los casos de uso de listar categorías, listar ingredientes,
>buscar por nombre, obtener imagen, mostrar detalles del cóctel y mostrar información del ingrediente
```mermaid
sequenceDiagram
    participant View
    participant Presenter
    participant Model
    participant Main
    actor User

    Main->>Presenter: Iniciar la aplicación
    Presenter->>Model: Obtener categorías
    Model->>Model: Realizar solicitud HTTP para obtener categorías
    Model-->>Presenter: Categorías
    Presenter->>Model: Obtener ingredientes
    Model->>Model: Realizar solicitud HTTP para obtener ingredientes
    Model-->>Presenter: Ingredientes
    Presenter->>View: Actualizar la vista con categorías e ingredientes
    Presenter->>View: Ejecutar la vista
    User->>View: Introducir nombre y pulsar search
    View->>Presenter: Solicitar nombres de cócteles
    Presenter->>Model: Obtener nombres de cócteles
    Model->>Model: Realizar solicitud HTTP para obtener cócteles
    Model-->>Presenter: Cócteles
    Presenter->>View: Mostrar cócteles
    User->>View: Seleccionar cóctel
    View->>Presenter: Solicitar detalles del cóctel
    Presenter->>Model: Obtener detalles del cóctel
    Model->>Model: Realizar solicitud HTTP para obtener detalles del cóctel
    Model-->>Presenter: Detalles del cóctel
    View->>Presenter: Solicitar imagen del cóctel
    Presenter->>Model: Obtener imagen del cóctel
    Model->>Model: Realizar solicitud HTTP para obtener imagen del cóctel
    Model-->>Presenter: Imagen del cóctel
    Presenter->>View: Mostrar detalles del cóctel con imagen
    User->>View: Seleccionar ingrediente del cóctel
    View->>Presenter: Solicitar información del ingrediente
    Presenter->>Model: Obtener información del ingrediente
    Model->>Model: Realizar solicitud HTTP para obtener información del ingrediente
    Model-->>Presenter: Información del ingrediente
    Presenter->>View: Mostrar información del ingrediente
```
>Diagrama de secuencia que muestra una situación en la que se mostraría el error de no enconntrado
```mermaid
sequenceDiagram
    participant View
    participant Presenter
    participant Model
    participant Main
    actor User

    Main->>Presenter: Iniciar la aplicación
    Presenter->>Model: Obtener categorías
    Model->>Model: Realizar solicitud HTTP para obtener categorías
    Model-->>Presenter: Categorías
    Presenter->>Model: Obtener ingredientes
    Model->>Model: Realizar solicitud HTTP para obtener ingredientes
    Model-->>Presenter: Ingredientes
    Presenter->>View: Actualizar la vista con categorías e ingredientes
    Presenter->>View: Ejecutar la vista
    User->>View: Introducir nombre no existente y pulsar search
    View->>Presenter: Solicitar nombres de cócteles
    Presenter->>Model: Obtener nombres de cócteles
    Model->>Model: Realizar solicitud HTTP para obtener cócteles
    Model-->>Presenter: Error cóctel no encontrado
    Presenter->>View: Mostrar mensaje de error de cóctel no encontrado
```
>Diagrama de secuencia que muestra una situación en la que se mostraría el error de conexión
```mermaid
sequenceDiagram
    participant View
    participant Presenter
    participant Model
    participant Main
    actor User

    Main->>Presenter: Iniciar la aplicación
    Presenter->>Model: Obtener categorías
    Model->>Model: Realizar solicitud HTTP para obtener categorías
    Model-->>Presenter: Categorías
    Presenter->>Model: Obtener ingredientes
    Model->>Model: Realizar solicitud HTTP para obtener ingredientes
    Model-->>Presenter: Ingredientes
    Presenter->>View: Actualizar la vista con categorías e ingredientes
    Presenter->>View: Ejecutar la vista
    User->>View: Introducir nombre del cóctel y pulsar search
    View->>Presenter: Solicitar nombres de cócteles
    Presenter->>Model: Obtener nombres de cócteles
    Model->>Model: Realizar solicitud HTTP para obtener cócteles
    Model-->>Presenter: Error de conexión
    Presenter->>View: Mostrar mensaje de error de conexión
```
>Diagrama de secuencia que muestra como se gestionaría el error de conexión si sucede al iniciar la aplicación
```mermaid
sequenceDiagram
    participant View
    participant Presenter
    participant Model
    participant Main
    participant Thread

    Main->>Presenter: Iniciar la aplicación
    Presenter->>Model: Obtener categorías
    Model->>Model: Realizar solicitud HTTP para obtener categorías
    Model-->>Presenter: Error de conexión
    Presenter->>View: Mostrar mensaje de error de conexión
    Presenter->>Thread: Cargar comboboxes
    loop Mientras no haya conexión
        Thread->>Model: Comprobar conexión
        Model-->>Thread: Estado de la conexión
    end
```
>Diagrama de secuencia que muestra una situación en la que se mostraría el error por tiempo de espera largo
```mermaid
sequenceDiagram
    participant View
    participant Presenter
    participant Model
    participant Main
    actor User

    Main->>Presenter: Iniciar la aplicación
    Presenter->>Model: Obtener categorías
    Model->>Model: Realizar solicitud HTTP para obtener categorías
    Model-->>Presenter: Categorías
    Presenter->>Model: Obtener ingredientes
    Model->>Model: Realizar solicitud HTTP para obtener ingredientes
    Model-->>Presenter: Ingredientes
    Presenter->>View: Actualizar la vista con categorías e ingredientes
    Presenter->>View: Ejecutar la vista
    User->>View: Seleccionar categoría
    View->>Presenter: Solicitar nombres de cócteles por categoría
    Presenter->>Model: Obtener nombres de cócteles por categoría
    Model->>Model: Realizar solicitud HTTP para obtener cócteles por categoría
    Model-->>Presenter: Error por tiempo de espera largo
    Presenter->>View: Mostrar mensaje de error por tiempo de espera largo
```
>Diagrama de secuencia que muestra una situación en la que se mostraría el error del servidor
```mermaid
sequenceDiagram
    participant View
    participant Presenter
    participant Model
    participant Main
    actor User

    Main->>Presenter: Iniciar la aplicación
    Presenter->>Model: Obtener categorías
    Model->>Model: Realizar solicitud HTTP para obtener categorías
    Model-->>Presenter: Categorías
    Presenter->>Model: Obtener ingredientes
    Model->>Model: Realizar solicitud HTTP para obtener ingredientes
    Model-->>Presenter: Ingredientes
    Presenter->>View: Actualizar la vista con categorías e ingredientes
    Presenter->>View: Ejecutar la vista
    User->>View: Seleccionar ingrediente
    View->>Presenter: Solicitar nombres de cócteles por ingrediente
    Presenter->>Model: Obtener nombres de cócteles por ingrediente
    Model->>Model: Realizar solicitud HTTP para obtener cócteles por ingrediente
    Model-->>Presenter: Error del servidor
    Presenter->>View: Mostrar mensaje de error del servidor
```
