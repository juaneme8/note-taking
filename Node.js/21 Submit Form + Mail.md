# Formulario al Mail (MailGun)
> Se usó como guía el video de [https://www.youtube.com/watch?v=JpcLd5UrDOQ](https://www.youtube.com/watch?v=JpcLd5UrDOQ)  con algunas simplificaciones basadas en el código de ejemplo de la documentación de MailGun.

Luego de investigar cómo llevar a cabo esta tarea vimos soluciones que utilizan Nodemailer, Nodemailer+Mailgun y la que implementaremos a continuación utilizando sólo MailGun.


>  En un comentario de Ian Schoonover en [este](https://www.youtube.com/watch?v=3bxjvequldY) video:
>  Nodemailer is a package that interfaces with all different API's, including Gmail SMTP. So it can use mailgun, which is a mail sending service that has an API. However, mailgun also has it's own npm package, so nodemailer isn't needed, unless you prefer it.

Mailchimp en cambio es para gente que se suscribe a contenido de nuestra página web.

Tenemos que crear una cuenta Free/ en [mailgun.com/](https://www.mailgun.com/) (destildar la opción de cargar tarjeta de crédito). Luego debemos agregar el correo donde queremos recibir los mails como *Authorized Recipients*, confirmando luego desde el mail. A continuación debemos copiar el *domain* y la *Private API Key* que luego usaremos en la aplicación Node.js.

`npm init -y` Crear archivos `server.js` donde tendremos la aplicación express y los endpoint handlers y `mail.js` donde exportaremos la función encargada de enviar el mail. 

`npm i express path mailgun-js`

# `server.js`
```js
const sendMail = require('./mail');
const express = require('express');
const app = express();
const path = require('path');

const PORT = 3000;

// Data parsing
app.use(express.urlencoded({ extended: false }));
app.use(express.json());

app.get('/', (req, res) => {
	res.sendFile(path.join(__dirname, 'views', 'index.html'));
});

app.post('/email', (req, res) => {
	const { nombre, email, tel, texto } = req.body;
	const subject = '📌 Nuevo Contacto: ' + nombre + ' (Tel: ' + tel + ')';
	const mailBody = 'Nombre: ' + nombre + ' | Correo Electrónico: ' + email + ' | Tel: ' + tel + ' | Texto:' + texto;

	sendMail(email, subject, mailBody, function (err, data) {
		if (err) {
			res.status(500).json({ message: 'Internal Error' });
		} else {
			console.log(data);
			res.redirect('/');
		}
	});
});

app.listen(PORT, () => {
	console.log('Escuchando en PORT', PORT);
});

```

# `mail.js`
```js
const api_key = '09d3c62485060253effd1347469aa66d-07e45e2a-882c5d8b';
const domain = 'sandboxcc6bee6f312941b6a99aaabd9472ea4e.mailgun.org';
const mailgun = require('mailgun-js')({ apiKey: api_key, domain: domain });

const sendMail = (email, subject, text, cb) => {
	const data = {
		from: 'hola@metanoia.com',
		to: 'metanoiatrad@gmail.com',
		subject,
		text,
	};
	mailgun.messages().send(data, cb);
};

module.exports = sendMail;
```

En vez de la propiedad `text` podemos usar `html` y directamente pasarle código html: `html: <h1 style="color: red">${text}</h1>`

Podemos encontrar información sobre el uso de templates en:  [https://documentation.mailgun.com/en/latest/user_manual.html#templates](https://documentation.mailgun.com/en/latest/user_manual.html#templates)