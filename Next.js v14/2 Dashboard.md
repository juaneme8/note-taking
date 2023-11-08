

# Admin Dashboard Tutorial

:link: Basado en el [tutorial de Lama Dev](https://youtu.be/cBg6xA5C60s) - [Repositorio](https://github.com/safak/nextadmin)



## Introducción

Para el siguiente tutorial utilizaremos Next.js 14

## Instalación Next.js

```
npx create-next-app@latest 
```



## Routes

La idea es tener tres páginas la Homepage, Dashboard y Login para eso creamos los siguientes archivos en el directorio `app`.

* `page.jsx`
* `dashboard/page.jsx`
* `login/page.jsx`



## Componentes

La documentación actual de Next.js utiliza el directorio `ui` para colocar allí los componentes a utilizar, crearemos un directorio para los componentes asociados a cada sección:

```
├── app/
│   ├── ui
│       ├── homepage
|       ├── login
|       └── dashboard
|           ├── navbar
|               ├── navbar.jsx
|           ├── sidebar
|               ├── sidebar.jsx
```



## Layout

Dentro del dashboard queremos contar con el mismo Sidebar y Navbar en varias secciones (creación edición, etc) los haremos formar parte del Layout.

```
├── app/
│   ├── dashboard
│       ├── page.jsx
|       └── layout.jsx
```

