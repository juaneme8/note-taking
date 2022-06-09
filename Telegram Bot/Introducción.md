# Telegram Bot

> Basado en el [video](https://youtu.be/Z7aN0xfn5NM) de Fazt Code



## Creación del Bot

En Telegram Desktop y buscar BotFather y hacer click en START. Veremos un listado de los comandos.



```
/newbot
```

Luego nos pedirá que le asignemos un nombre y un nombre de usuario terminado en bot.

En la [documentación](https://core.telegram.org/bots/api) podemos ver cómo trabajar con la API de manera directa, sin embargo también existen [bibliotecas](https://core.telegram.org/bots/samples) que resuelven esto de manera más simple.

Utilizaremos una de ellas llamada [Telegraf](https://telegraf.js.org/)

A continuación mostramos como iniciar el bot y crear un comando personalizado `/check`.

```js
const {Telegraf} = require('telegraf');

const bot = new Telegraf(process.env.BOT_TOKEN);

bot.start(ctx => {
  ctx.reply('¡Bienvenido al Bot!');
});

bot.command('check', async ctx => {
  await main();
  ctx.reply(`🚨 Últimos Movimientos`);
});
```

