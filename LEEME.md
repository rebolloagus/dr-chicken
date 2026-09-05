# Dr Chicken · App de pedidos

### 👉 https://rebolloagus.github.io/dr-chicken/

App web para administrar los pedidos del delivery (viernes a domingo, 20:30 a 23:30).
Todo vive en un solo archivo: `index.html`.

Se adapta sola: en la laptop muestra los pedidos en columnas con el menú arriba, y en
el celular queda como una app con las pestañas abajo.

## Cómo se usa

**Pedidos** — la noche en curso. Botón amarillo *Pedido* para cargar uno nuevo:
cliente, teléfono, dirección, los box tocando el precio, las salsas incluidas de
cada box, notas y cómo paga. Después cada pedido tiene dos botones:

- **💬** le manda al cliente el aviso de que sale, por WhatsApp. En la laptop abre
  WhatsApp Web derecho en el chat, siempre en la misma pestaña. El texto se edita
  en Ajustes y admite `{cliente}`, `{total}` y `{pedido}`.
- **Entregado** lo cierra.

**Caja** — tiene dos vistas:

- *Por noche*: la ganancia, cuánto entró en efectivo y cuánto por transferencia, los
  costos que cargues (pollo, papas, aceite, nafta, envases), qué box se vendieron, y
  el botón **Terminar la jornada**, que avisa si quedan pedidos sin entregar o sin
  forma de pago antes de cerrar. Una noche cerrada se puede reabrir cuando quieras.
- *Por mes*: todas las noches del mes sumadas, noche por noche, con el ranking de qué
  box se vendió más — en unidades y en plata. Es la caja mensual.

Cerrar una jornada no congela ni borra nada: los números se siguen calculando de los
pedidos reales, y si cambian después del cierre la pantalla lo avisa.

**Menú** — los precios. Tocá cualquier producto para cambiarle el precio, la
descripción o esconderlo. Los cambios de precio no tocan los pedidos ya cargados.

> La noche se cierra a las 12:00 del día siguiente, así un pedido cargado 00:30
> sigue contando para la noche del viernes y no para la del sábado.

## Sincronizar los dos celulares

Sin configurar nada la app funciona perfecto, pero cada celular guarda sus propios
pedidos. Para que los dos vean lo mismo al instante:

1. Entrar a **console.firebase.google.com** con la cuenta de Google del negocio y
   crear un proyecto (el plan gratis alcanza de sobra; se puede saltear Analytics).
2. Dentro del proyecto: **Compilación → Firestore Database → Crear base de datos**,
   modo producción, ubicación `southamerica-east1`.
3. En **Configuración del proyecto** (el engranaje) → *Tus apps* → ícono `</>` para
   registrar una app web (sin marcar Hosting). Firebase muestra un bloque de código:
   copiarlo entero, tal cual, no hace falta acomodar nada.
4. En la app: engranaje arriba a la derecha → pegar eso en *Configuración de
   Firebase*, poner el mismo **código del negocio** en todos los dispositivos y tocar
   *Conectar*. El puntito del header se pone verde y dice "En línea".

   El código del negocio es la llave de la base: inventá uno largo y difícil de
   adivinar, y **no lo publiques en ningún lado**. Acá va como `TU-CODIGO-SECRETO`
   justamente porque este archivo es público.
5. En **Firestore → Reglas**, pegar esto y publicar, con el código del paso 4 en
   lugar de `TU-CODIGO-SECRETO`:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /negocios/{negocio}/{documento=**} {
      allow read, write: if negocio == 'TU-CODIGO-SECRETO';
    }
  }
}
```

Sin ese último paso la base queda abierta a cualquiera. El código del negocio hace
de llave, por eso conviene que sea largo: nunca se publica en el código de la app,
se escribe a mano en cada dispositivo.

## Instalarla en el celular

- **iPhone:** abrir el link **en Safari** → botón Compartir → *Añadir a pantalla de inicio*.
- **Android:** abrirlo en Chrome → los tres puntos → *Agregar a pantalla de inicio*.

Queda con ícono propio y se abre en pantalla completa, como una app cualquiera.

> No la uses desde el mini-navegador que abre WhatsApp adentro del chat: ese borra
> los datos guardados al cerrarse. Copiar el link y pegarlo en Safari o Chrome.

## Respaldo

Ajustes → *Exportar datos* baja un `.json` con todo. *Importar datos* lo vuelve a
cargar. Conviene hacerlo cada tanto si no están usando la sincronización.
