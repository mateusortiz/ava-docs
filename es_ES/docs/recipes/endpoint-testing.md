___
**Nota del traductor**

Esta es la traducción del archivo [endpoint-testing.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/endpoint-testing.md). Aquí hay un [enlace](https://github.com/sindresorhus/ava/compare/37e0dd84d25a37ce8eaf907094652bc28e8d0289...master#diff-aee54ab6a703c02779edb3ebbb35e96f) a las diferencias con la rama master de AVA (Si al hacer clic en el enlace no se encuentran modificaciones en el archivo `endpoint-testing.md`, será por que la traducción está actualizada).
___
# Testando un Endpoint

AVA no trae un método integrado para testar endpoints, pero puedes usar cualquier biblioteca de aserción para ello. Vamos a usar [`supertest-as-promised`](https://github.com/WhoopInc/supertest-as-promised).

Puesto que las pruebas se ejecutan concurrentemente, es mejor crear una instancia de servidor nueva para cada test, porque si hace referencia a la misma instancia podría mutar entre tests. Esto se puede lograr con 'test.beforeEach' y 't.context', o simplemente con una factoría:

```js
function makeApp() {
	const app = express();
	app.post('/signup', signupHandler);
	return app;
}
```

A continuación, tan solo inyecta tu instancia de servidor en el supertest. El único consejo es usar una promesa o la sintaxis async/await en vez del método `end` de supertest:

```js
test('signup:Success', async t => {
	t.plan(2);

	const res = await request(makeApp())
		.post('/signup')
		.send({email: 'ava@rocks.com', password: '123123'});

	t.is(res.status, 200);
	t.is(res.body.email, 'ava@rocks.com');
});
```
