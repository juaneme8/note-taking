# Menu Programático
Definimos `LINKS` como un array de objetos donde tendremos los elementos a mostrar en el menú. Como estamos trabajando con TypeScript indicamos el tipo de datos que almacenará dicho array y lo hacemos mediante una interface `ListLink`.

De acuerdo a la ruta en la cual nos encontremos queremos resaltar el ícono de esa sección con `activeIcon` y cambiarle el color. Para eso hacemos uso del hook de React Router DOM llamado `useLocation`, luego `const {pathname} = useLocation();` y luego dentro del mapeo usamos el operador ternario para definir estas variantes:

`color={pathname === link.href ? "primary.500" : "inherit"}` y para el ícono `<Icon as={pathname === link.href ? link.activeIcon : link.inactiveIcon} height={6} width={6}/>`

```jsx
import {Link, useLocation} from "react-router-dom";

interface ListLink {
  href: string;
  text: string;
  activeIcon: IconType;
  inactiveIcon: IconType;
}

const LINKS: ListLink[] = [
  {
    href: "/home",
    text: "Inicio",
    activeIcon: BsHouseFill,
    inactiveIcon: BsHouse,
  },
  {
    href: "/explore",
    text: "Explorar",
    activeIcon: BsHash,
    inactiveIcon: BsHash,
  },
  {
    href: "/notifications",
    text: "Notificaciones",
    activeIcon: BsBellFill,
    inactiveIcon: BsBell,
  },
  {
    href: "/messages",
    text: "Messages",
    activeIcon: BsEnvelopeFill,
    inactiveIcon: BsEnvelope,
  },
  {
    href: "/bookmarks",
    text: "Guardados",
    activeIcon: BsBookmarkFill,
    inactiveIcon: BsBookmark,
  },
  {
    href: "/lists",
    text: "Listas",
    activeIcon: BsList,
    inactiveIcon: BsList,
  },
  {
    href: "/profile",
    text: "Perfil",
    activeIcon: BsPersonFill,
    inactiveIcon: BsPerson,
  },
  {
    href: "/options",
    text: "Mas opciones",
    activeIcon: BsThreeDots,
    inactiveIcon: BsThreeDots,
  },
];

const Navbar: React.FC<StackProps> = (props) => {
  const {pathname} = useLocation();

  return (
    <Stack fontSize="xl" fontWeight="bold" spacing={7} width="100%" {...props}>
      {LINKS.map((link) => (
        <Link key={link.href} to={link.href}>
          <Stack
            alignItems="center"
            color={pathname === link.href ? "primary.500" : "inherit"}
            direction="row"
            spacing={5}
          >
            <Icon
              as={pathname === link.href ? link.activeIcon : link.inactiveIcon}
              height={6}
              width={6}
            />
            <Text>{link.text}</Text>
          </Stack>
        </Link>
      ))}
    </Stack>
  );
};

export default Navbar;

```

==StackProps==