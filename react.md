# REACT

##### FROM ZERO TO HERO

---

## `PROMESAS`

Las promesas se crean con:

- palabra reservada 'new'
- palabra reservada Promise (la p con mayúscula)
- con un argumento adentro, que es un callback
- el callback recibe dos argumentos: resolve y reject
- se los llama asi por convención, realmente los puedes llamar como te de la gana
- resolve se ejecuta cuando la promesa es exitosa
- reject se ejecuta cuando ha habido un error
- setTimeout es una funcion que recibe dos argumentos. Un callback y el tiempo en que ejecutes ese callback.
- En este caso, el console log, lo hará despues de 2000 milisegundos

```javascript
const promesa = new Promise((resolve, reject) => {
  setTimeout(() => {
    console.log('2 segundos después');
  }, 2000);
});
```

Con ese código, se habrá corrido el resto de código en caso de existir, y dos segundos despues, al final, se cumple tu promesa. Ahora, como hacer para ejecutar código DESPUES de que se cumpla la promesa?
Puedes dejar 'automatizado' eso con disntintos metodos:

- catch()
- finally()
- then()

### `then()`

`then()` lo que pones dentro de then() es lo que se ejecutará una vez que se cumpla la promesa. Then() recibe como parámtro un callback

#### ejemplo:

```javascript
promesa.then(() => {
    console.log('Then de la promesa)
})
```

Ese código de arriba lo escribirías después de declarar la promesa. El código total te quedaría todo así:

```javascript
const promesa = new Promise((resolve, reject) => {
  setTimeout(() => {
    console.log('2 segundos después');
  }, 2000);
});

promesa.then(() => {
    console.log('Then de la promesa)
})
```

Hay un problema con el código de arriba, y es que no se le esta especificando al then absolutamente nada.
Es por eso que al declarar la promesa, ahí tienes que ejecutar el resolve(), al cual le tienes que pasar un parámetro, y por defecto, este se lo pasa al then() que está en el bloque de código de abajo.

Asumamos que en vez de un console log, estas haciendo una solicitud a una API, y eso obviamente lo guardas en una const.
En vez de hacer un console log de ese const en el código de la promesa, puedes ahi escribir un resolve, el cual recibe como parámetro, la const que contiene a esa petición de API.

Esa const es luego pasada por el resolve() al then(), el cual puede ejecutar cualquier linea de código.

#### ejemplo:

```javascript
const promesa = new Promise((resolve, reject) => {
  setTimeout(() => {
    const heroe = estaEsTuPeticionApi(4);
    resolve(heroe);
  }, 2000);
});

promesa.then((heroe) => {
  console.log(heroe);
});
```

Ahora si, con ese código, estas ejecutando asincronismo :)

<br>
<br>

### `catch()`

`cacth()` lo que pones dentro, se ejecutara cuando la promesa devuelva un error

`finally()` se ejecuta despues del then o despues del catch, es decir, ya sea que se haya ejecutado cualquier de esos dos, el finally() es lo que se ejecuta despues de eso.
