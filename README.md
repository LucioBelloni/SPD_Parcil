# 📋 Primer Parcial SPD
![Logo de GitHub](https://github.com/ErikLujan/proyecto/blob/main/Imagen/arduino.jpeg)

# <h2>👥 Integrantes del grupo</h2>
- Lucio Belloni
- Ciro Luongo
- Erik Lujan
# <h2>📌 Parte 1: Contador de 0 a 99 con Display de 7 segmentos y multiplexación</h2>
![Logo de GitHub](https://github.com/ErikLujan/proyecto/blob/main/Imagen/Contador%20de%200%20a%2099%20Arduino.png)

# <h2>📎 Descripcion del proyecto</h2>
Este circuito cumple con la funcion de, mediante displays de 7 segmentos, mostrarnos numeros desde el 0 hasta el 99. Este contador se maneja mediante 3 pulsadores y cada uno tiene su respectiva funcion: Uno para subir el numero en el contador, otro para bajarlo y el ultimo para reiniciar el contador e inicializarlo en 0.

# <h2>Funcion principal (loop)</h2>
Esta funcion cumple con las siguientes caracteristicas:
- Lee el estado de los botones para determinar si se presionó alguno.
- Actualiza el contador en función de los botones presionados.
- Llama a MostrarContador() para mostrar el valor actual en los displays.
-  No toma parámetros y no retorna ningún valor.
```
void loop()
{
  int presionado = botonPresionado();
  if (presionado == botonSubir) 
  {
    contador++;
    if (contador > 99)
    {
      contador = 0;
    } 
  }
  else if(presionado == botonBajar)
  {
    contador--;
    if (contador < 0)
    {
      contador = 99;
    }
  }
  else if(presionado == botonReset)
  {
    contador = 0;
  }
  MostrarContador(contador);
}
```
# <h2>Funcion ApagarDisplay()</h2>
Esta funcion se encarga de apagar todos los leds de los displays, apagando los pines A, B, C, D, E, F y G. No toma parámetros ni retorna ningún valor.
```
void ApagarDisplay()
{
  apagarLeds(A);
  apagarLeds(B);
  apagarLeds(C);
  apagarLeds(D);
  apagarLeds(E);
  apagarLeds(F);
  apagarLeds(G);
}
```

# <h2>Funcion MostrarDigito()</h2>
- Muestra un dígito específico en los displays. Toma un parámetro digito que es el número a mostrar (0-9).
- Enciende los leds correspondientes para mostrar el dígito.
```
void MostrarDigito(int digito)
{
  ApagarDisplay();
  switch(digito)
  {
    case 0:
      numeroCero();
      break;
    case 1:
      numeroUno();
      break;
    case 2:
      numeroDos();
      break;
    case 3:
      numeroTres();
      break;
    case 4:
      numeroCuatro();
      break;
    case 5:
      numeroCinco();
      break;
    case 6:
      numeroSeis();
      break;
    case 7:
      numeroSiete();
      break;
    case 8:
      numeroOcho();
      break;
    case 9:
      numeroNueve();
      break;
  }
}
```

# <h2>Funcion prenderDigito()</h2>
Controla cuál de los dos displays (UNIDAD o DECENA) debe estar encendido y cuál apagado. Toma como un parámetro digito que indica qué display se debe prender.
```
void prenderDigito(int digito)
{
  if (digito == UNIDAD)
  {
    digitalWrite(UNIDAD, LOW);
    digitalWrite(DECENA, HIGH);
  }
  else if (digito == DECENA)
  {
    digitalWrite(UNIDAD, HIGH);
    digitalWrite(DECENA, LOW);
  }
  else
  {
    digitalWrite(UNIDAD, HIGH);
    digitalWrite(DECENA, HIGH);
  }
}
```

# <h2>Funcion MostrarContador()</h2>
- Muestra el valor del contador en los displays. Toma un parámetro contador que es el número que se mostrará en los displays.
- Divide el número en unidades y decenas y muestra ambos dígitos en los displays.

```
void MostrarContador(int contador)
{
  ApagarDisplay();
  prenderDigito(UNIDAD);
  MostrarDigito(contador % 10);
  delay(5);
  ApagarDisplay();
  prenderDigito(DECENA);
  MostrarDigito(contador / 10);
  delay(tiempo);
}
```

# <h2>Funcion botonPresionado()</h2>
- Lee el estado de los botones y detecta si alguno de ellos ha sido presionado desde la última llamada.
- No toma parámetros.
- Retorna un valor que representa el botón presionado (botonSubir, botonBajar, botonReset) o 0, en caso de que ningún botón sea presionado.
```
int botonPresionado(void)
{
  int lecturaSube = digitalRead(botonSubir);
  int lecturaBaja = digitalRead(botonBajar);
  int lecturaReset = digitalRead(botonReset);
  
  if(lecturaSube == 1)
  {
    subePrevia = 1;
  }
  if(lecturaBaja == 1)
  {
    bajaPrevia = 1;
  }
  if(lecturaReset == 1)
  {
    resetPrevia = 1;
  }
  
  if(lecturaSube == 0 && lecturaSube != subePrevia)
  {
    subePrevia = lecturaSube;
    return botonSubir;
  }
  if(lecturaBaja == 0 && lecturaBaja != bajaPrevia)
  {
    bajaPrevia = lecturaBaja;
    return botonBajar;
  }
  if(lecturaReset == 0 && lecturaReset != resetPrevia)
  {
    resetPrevia = lecturaReset;
    return botonReset;
  }
 return 0;
}
```

# <h2>Funcion encenderLeds()</h2>
Enciende el led conectado al pin correspondiente. Toma como parámetro un pin que es el número del pin al que se debe aplicar la señal de encendido.
```
void encenderLeds(int pin)
{
  digitalWrite(pin, HIGH);
}
```

# <h2>Funcion apagarLeds()</h2>
Apaga el led conectado al pin correspondiente. Toma como parámetro un pin que es el número del pin al que se debe aplicar la señal de apagado.
```
void apagarLeds(int pin)
{
  digitalWrite(pin, LOW);
}
```

# <h2>Funciones de numeroCero() a numeroNueve()</h2>
- Son funciones que encienden los leds en patrones específicos para mostrar los números del 0 al 9 en los displays.
- No toman parámetros ni retornan valores. Cada función está diseñada para mostrar un número específico en los displays.
```
void numeroUno(void){
  apagarLeds(A);
  apagarLeds(D);
  apagarLeds(E);
  apagarLeds(F);
  apagarLeds(G);
  encenderLeds(B);
  encenderLeds(C);
}

void numeroDos(void){
  apagarLeds(C);
  apagarLeds(F);
  encenderLeds(A);
  encenderLeds(B);
  encenderLeds(D);
  encenderLeds(E);
  encenderLeds(G); 	
}

void numeroTres(void){
  apagarLeds(E);
  apagarLeds(F);	
  encenderLeds(A);
  encenderLeds(B);
  encenderLeds(C);
  encenderLeds(D);
  encenderLeds(G);
}

void numeroCuatro(void){
  apagarLeds(A);
  apagarLeds(D);
  apagarLeds(E);
  encenderLeds(B);
  encenderLeds(C);
  encenderLeds(F);
  encenderLeds(G);
}

void numeroCinco(void) {
  apagarLeds(B);
  apagarLeds(E);
  encenderLeds(A);
  encenderLeds(C);
  encenderLeds(D);
  encenderLeds(F);
  encenderLeds(G);
}

void numeroSeis(void) {
  apagarLeds(B);
  encenderLeds(A);
  encenderLeds(C);
  encenderLeds(D);
  encenderLeds(E);
  encenderLeds(F);
  encenderLeds(G);
}

void numeroSiete(void){
  apagarLeds(D);
  apagarLeds(E);
  apagarLeds(F);
  apagarLeds(G);
  encenderLeds(A);
  encenderLeds(B);
  encenderLeds(C);
}

void numeroOcho(void){
  encenderLeds(A);
  encenderLeds(B);
  encenderLeds(C);
  encenderLeds(D);
  encenderLeds(E);
  encenderLeds(F);
  encenderLeds(G);
}

void numeroNueve(void) {
  apagarLeds(E);
  encenderLeds(A);
  encenderLeds(B);
  encenderLeds(C);
  encenderLeds(D);
  encenderLeds(F);
  encenderLeds(G);
}
```

# <h2>👾 Link al Proyecto</h2>
- [Proyecto (Parte 1)](https://www.tinkercad.com/things/86LzXQmuniS)

# <h2>📌 Parte 2: Modificación con Interruptor Deslizante y Números Primos</h2>
![Logo de GitHub](https://github.com/ErikLujan/proyecto/blob/main/Imagen/Modificacion%20con%20Interruptor.png)

# <h2>📎 Descripcion del proyecto</h2>
Este circuito es una modificacion del anterior, con la diferencia de que en lugar de manejarse con pulsadores, ahora se maneja con un interruptor deslizante (switch). El cual si se encuentro del lado izquierdo (HIGH), se mostrara el contador normal que va de 0 a 99. Caso contrario, si el interruptor esta del lado derecho (LOW), solo me mostrara los numeros primos que hay entre 0 y 99.

# <h2>Cambios en la funcion principal (loop)</h2>
A diferencia de la parte 1, aca hubo unos cuantos cambios:
- Lee el estado del interruptor deslizante y almacena su valor en interruptorEstado.
- Realiza una lectura analógica de un sensor de temperatura y guarda su valor en lecturaTemp.
- Compara la temperatura con un valor determinado (en este caso 60, representando los grados celsius). Si la temperatura es mayor que este, el contador se reinicia a cero.
- Si la temperatura está por debajo del valor, entonces se procede a verificar el estado del interruptor. Si esta en HIGH, el modo de visualizacion cambia y mostrara el contador con todos los numeros del 0 al 99. Caso contrario, su modo de visualizacion sera solo para los numeros primos.
```
void loop()
{
  int presionado = botonPresionado();
  interruptorEstado = digitalRead(INTERRUPTOR);

  int lecturaTemp;
  int temperatura;
  lecturaTemp = analogRead(TEMPERATURA);
  temperatura = map(lecturaTemp, 20, 358, -40, 125);
  
  if (temperatura > 60)
  {
    mostrarSecuenciaAleatoria();
  }
  else 
  {
    if (interruptorEstado != interruptorEstadoPrevia)
    {
      interruptorEstadoPrevia = interruptorEstado;
      modoPrimos = (interruptorEstado == LOW);
      contador = 0;
    }
    if (!modoPrimos)
    {
      if (presionado == botonSubir)
      {
        contador++;
        if (contador > 99)
        {
          contador = 0;
        }
      }
      else if (presionado == botonBajar)
      {
        contador--;
        if (contador < 0)
        {
          contador = 99;
        }
      }
    }
    else
    {
      if (presionado == botonSubir)
      {
        do {
          contador++;
        } while (!esPrimo(contador) && contador <= 99);
        if (contador > 99)
        {
          contador = 0;
        }
      }
      else if (presionado == botonBajar)
      {
        do {
          contador--;
        } while (!esPrimo(contador) && contador >= 0);
        if (contador < 0)
        {
          contador = 97;
        }
      }
    }
  }
  MostrarContador(contador);
}
```

# <h2>Cambios en la funcion botonPresionado()</h2>
El unico cambio que se ha realizado en esta funcion, fue eliminar la lectura del reset y el if que verificaba el estado del botonReset, al igual que su retorno. Ya que se reemplazo este boton por el interruptor. El resto sigue funcionando igual.
```
int botonPresionado(void)
{ 
  int lecturaSube = digitalRead(botonSubir);
  int lecturaBaja = digitalRead(botonBajar);
  
  if (lecturaSube == 1)
  {
    subePrevia = 1;
  }

  if (lecturaBaja == 1)
  {
    bajaPrevia = 1;
  }
  
  if (lecturaSube == 0 && lecturaSube != subePrevia)
  {
    subePrevia = lecturaSube;
    return botonSubir;
  }
  
  if (lecturaBaja == 0 && lecturaBaja != bajaPrevia)
  {
    bajaPrevia = lecturaBaja;
    return botonBajar;
  }
  return 0;
}
```

# <h2>Funcion mostrarSecuenciaAleatoria</h2>
Esta funcion se encarga de generar una especie de parpadeo en el display, apagando y prendiendo cada pin por un determinado tiempo. La idea de esta funcion fue utilizarla como un "pausador" del contador, es decir, si se la temperatura es superior al valor dado, el contador se va a pausar.
```
void mostrarSecuenciaAleatoria(void)
{
    encenderLeds(A);
    encenderLeds(F);
    encenderLeds(E);
    encenderLeds(D);
    encenderLeds(C);
    encenderLeds(B);
    encenderLeds(G);
    encenderLeds(UNIDAD);
    encenderLeds(DECENA);
    delay(100);
    apagarLeds(A);
    apagarLeds(F);
    apagarLeds(E);
    apagarLeds(D);
    apagarLeds(C);
    apagarLeds(B);
    apagarLeds(G);
    apagarLeds(UNIDAD);
    apagarLeds(DECENA);
    delay(100);
}
```

# <h2>Funcion esPrimo()</h2>
- Esta es una función de tipo booleana que verifica si el número dado es primo.
- Toma un número como entrada y devuelve true si es primo y false si no lo es.
```
bool esPrimo(int numero) {
  if (numero <= 1) {
    return false;
  }
  for (int i = 2; i * i <= numero; i++) {
    if (numero % i == 0) {
      return false;
    }
  }
  return true;
}
```

# <h2>👾 Link al Proyecto</h2>
- [Proyecto (Parte 2)](https://www.tinkercad.com/things/ayVbRWMEuxx)
