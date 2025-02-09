+++
date = "2015-03-20"
title = "Examen Programación Hilos - HinchaGlobos y PinchaGlobos"
author = "Javier Olmedo"
+++

[![/images/examen-programacion-hilos-hinchaglobos-y-pinchaglobos/examen-programacion-hilos-hinchaglobos-y-pinchaglobos_banner.png](/images/examen-programacion-hilos-hinchaglobos-y-pinchaglobos/examen-programacion-hilos-hinchaglobos-y-pinchaglobos_banner.png)](/images/examen-programacion-hilos-hinchaglobos-y-pinchaglobos/examen-programacion-hilos-hinchaglobos-y-pinchaglobos_banner.png)

Recientemente acabo de **terminar mis exámenes** de programación, y en este caso me gustaría compartir con vosotros el examen de **programación de servicios y procesos**.

El **lenguaje de programación** que se ha utilizado para **resolver** el ejercicio es **Java**, y el examen decía lo siguiente (Dejo la nota de cada punto por si queréis intentarlo vosotros y calcular cual sería vuestra nota):

## Examen Programación Servicios y Procesos
```
Hacer un programa compuesto por cuatro clases:
• Clases hilos HinchaGlobos (HG) (encargada de hinchar los globos)
• Clase hilo PinchaGlobos (PG) (encargada de pinchar los globos)
• Clase Globos (almacén de globos)
• Clase Inicio (contiene el código main e instancia al resto)

En relación a la clase Globos (tendremos 1 instancia de esta clase)
• En el almacén habrá un máximo de 10 globos .
• Se irán entregando de uno en uno numerándolos desde 1 a 10.
• Una vez entregados se podrán hinchar hasta que estallen o los pinchen.
• Sólo podrá haber 3 globos hinchándose a la vez.
• Los globos tendrán volumen de 1 a 5. Si se llena más de 5 estallan.
• El globo se entrega con un volumen inicial de l.
• Los globos pueden ser pinchados mientras se están hinchando.
• Se escribirá un mensaje con el número de globo cada vez que:
o Se entregue un globo (indicaremos el nombre del hilo ejem: GLOBO 5 ENTREGADO A HG3) o Se hinche un globo e indicaremos el nuevo volumen (ejem: GLOBO 5 VOLUMEN 5)
o Se estalle un globo por inflar de más (ejem : GLOBO 5 ESTALLA)
o Un PG pinche un globo (indicaremos el nombre del hilo ejem : GLOBO 5 PINCHADO POR PG3)

En relación a la clase HinchaGlobos (tendremos 5 instancias de esta clase)
• (0,5) Obtendrá un globo de la clase globos cada vez y en orden salvo que no queden más.
• (2) Si ya hubiera tres hinchándose se esperará hasta que uno se pinche o estalle .
• Los tendremos que numerar y nombrar con HG seguido de su número. (ejem: HG3)
• (1) Intentará hincharlo hasta estallarlo salvo que sea pinchando.
• (1) Cada segundo aumentará el volumen del globo desde 1 hasta estallarlo.
• (1) Si estalla o se pincha volverá a por otro globo hasta que no queden más.

En relación a la clase PinchaGlobos (tendremos 5 instancias de esta clase)
• (1) Intentará pinchar uno de los globos que se está hinchando cada un tiempo aleatorio de 1 a 10 segundos .
• (1) Si no hay globos que pinchar se quedará en espera.
• (0,5) Dejará de pinchar cuando no queden globos que pinchar.
• Sus instancias las tendremos que numerar y nombrar con PG seguida de su número (ejem : PG 2)

Los distintos apartados serán comprobados por la impresión de los mensajes detallados en la clase Globos .

Se deberá tener en cuenta que aquellos dormidos deberán ser despertados convenientemente:
(1) Caso de HG dormidos
(1) Caso de PG dormidos
```

A continuación **adjunto las 4 clases**, lo que sería el **examen perfecto** con nota de **10**, he intentado **comentar** todo para que podáis seguir el examen, si no entendéis algo lo modificaré.

## 1 - Clase Inicio (Main)

```java
public class Inicio {
 
    public static void main(String[] args) {
 
        Globos g = new Globos();    //Instanciamos Globos
 
        for (int i=1;i<=5;i++) new HinchaGlobo(g,i);    //Bucle que crea 5 hilos de HinchaGlobo, le pasamos los globos e i para saber el número del hilo
        for (int i=1;i<=5;i++) new PinchaGlobo(g,i);    //Bucle que crea 5 hilos de PinchaGlobo, le pasamos los globos e i para saber el número del hilo
    }
}
```

## 2 - Clase Globos (Almacén de globos y los métodos)

```java
public class Globos {
 
    private int maxHinchando=3;     // Número máximo de globos hinchando a la vez
    private int maxGlobos=10;        // Número máximo de globos en el almacén
    private int maxVolumen=5;          // Volumen máximo del globo
    private int nGlobo=1;             // Número del globo para dar, inicialmente 1
    private int hinchandoAhora=0;     // Cuántos se están hinchando ahora mismo
    private int globos[];
    // Si vale 0 el globo no está dado
    // Si vale 1 a maxVolumen está dado e indica su volumen
    // Si vale maxVolumen+1 está roto
    // Si vale maxVolumen+2 está pinchado
 
    public Globos() {    // Constructor de Globos
        globos=new int[maxGlobos];
        for (int i=0;i<maxGlobos;i++) globos[i]=0; // Rellenamos el almacén a 0, (ninguno dado).
    } 
 
    public synchronized int dameGlobo() { // Método que devuelve el siguiente globo del almacen, si no quedan devuelve -1
 
        while (hinchandoAhora==maxHinchando && nGlobo!=maxGlobos+1)  { // Me espero si ya hay maxHinchando y quedan globos por dar
            try {wait();} catch (Exception e) {}
        }
 
        if (nGlobo==maxGlobos+1) return -1; // Retorno un -1 si no quedan globos por dar
        globos[nGlobo-1]=1;                 // Cambio el 0 del almacen de globos por 1 (entregado a HinchaGlobos)
        System.out.println("GLOBO "+nGlobo+" ENTREGADO A "+Thread.currentThread().getName());    //Informo por consola a que hilo se le da el globo
        hinchandoAhora++;     //Sumo 1 al hinchaAhora
        notifyAll();        //Notifico a todos que hay un cambio
        return nGlobo++;    //Retorno el globo
    }
 
    public synchronized boolean pincho() { // Método que pincha un globo, si no quedan devuelve true
 
        while (hinchandoAhora==0 && nGlobo!=maxGlobos+1){    // Me espero si no hay hinchando y quedan por pinchar
            try {wait();} catch (Exception e) {}
        }
 
        if (nGlobo==maxGlobos+1) return true;    // Me aseguro de salir porque ha cambiado hinchando
 
        for (int i=0;i<maxGlobos;i++)    // Busco un globo para pinchar dentro del almacen
            if (globos[i]>0 && globos[i]<=maxVolumen) {
                System.out.println("GLOBO"+(i+1)+" LO PINCHA "+Thread.currentThread().getName());
                globos[i]=maxVolumen+2;
                hinchandoAhora--;
                notifyAll();
                break;
            }
 
        return hinchandoAhora!=0;
    }
 
    public synchronized boolean hincho(int num) {
 
        if (globos[num-1]<=maxVolumen) globos[num-1]++;    // Puede que ya esté estallado, se comprueba
        else return true;                                // si estuviera estallado, pinchado o estallase dev true
 
        if (globos[num-1]==maxVolumen+1){    // Si lo he estallado lo notifico
            hinchandoAhora--;
            System.out.println("GLOBO "+num+ " ESTALLA");
            notifyAll();
            return true;
        }
        else {
            System.out.println("GLOBO "+num+" VOLUMEN "+globos[num-1]);
            return false;
        }
    }
}
```

# 3 - Clase HinchaGlobo

```java
public class HinchaGlobo extends Thread {
    //Los globos estallan cuando pasan de 10
    private Globos g;
    private int numero;
 
    public HinchaGlobo(Globos pg,int pnumero) {
        g=pg;
        numero=pnumero;
        setName("HG"+numero);
        start();
    }
 
    @Override
    public void run(){
        int manejado;
        boolean estalla; // cierto si se pincha o estalla
 
        while (true) {
            if ((manejado=g.dameGlobo())==-1) break ; // me da un globo o -1 si no hay mas
            do{
                try {Thread.sleep(1000);} catch (Exception e) {}
                estalla=g.hincho(manejado);
            } while (!estalla);  
 
        } // while(true)
    } // public run
} // public class
```

## 4 - Clase PinchaGlobo

```java
public class PinchaGlobo extends Thread{
    private Globos g;
    private int numero;
    public PinchaGlobo(Globos pg,int pnumero){
        g=pg;
        numero=pnumero;
        setName("PG"+numero);
        start();
    }
 
    @Override
    public void run(){
        int num;
        boolean nohaymas;
        do{
            try {Thread.sleep((int)(Math.random()*5000));} catch (Exception e) {}
            // Si no quedan globos tengo que dejar de pinchar
            nohaymas=g.pincho();
        } while (!nohaymas); // while true
    } // public run
} // public class
```

**¿Que nota has sacado?**

Hasta aquí la entrada, no dudes en publicar tu nota.