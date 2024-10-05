
# Monitores

---
## 3
> Resolver el siguiente problema. En una montaña hay 30 escaladores que en una parte de la subida deben utilizar un único paso de a uno a la vez y de acuerdo con el orden de llegada al mismo. Nota: sólo se pueden utilizar procesos que representen a los escaladores; cada escalador usa sólo una vez el paso.

```c

Process Escalador::[1..30] {
	Montaña.entrar();
	delay(5000) // Pasa por la escalada
	Montaña.salir();
}

Monitor Montaña {
	bool pasando = false;
	int cantEsperando = 0;
	cond espera;

	Procedure entrar() {
		if (pasando) {
			cantEsperando += 1;
			wait(espera);
		} else {
			pasando = true;
		}
	}

	Procedure salir() {
		if (cantEsperando > 0) {
			esperando -= 1;
			signal(espera);
		} else {
			pasando = false;
		}
	}

}
```