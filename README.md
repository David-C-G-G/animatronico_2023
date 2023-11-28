# animatronico_2023
El código de un animatronico para cocotron 2023

Elementos Utilizados:
El animatronico llamado "Mictecacihuatl" fue programado usando los siguientes elementos electrónicos:
6 - Servo motores modelo: MG995 marca: Tower Pro

1 - Micro Servo motor modelo: SG90 marca: Tower Pro
2 - LED RGB
1 - Placav PCA9685
1 - Raspberry Pi 2 Model B.V1.1
1 - Arduino UNO Generico con integrado: ATMEL ATMEGA328P-PU
1 - sensor sharp
1 - sensor IR
cables tipo dupont para las conexiones, extensores de servomotores, y cables de alimentación para las placas

# Descripción de funcionalidad:
El animatrónico elaborado fue la representación del personaje "Mictecacihuatl" el cual representaba en diversas historias de la epoco prehispanica una deidad al lado de Mictlantecutli, Mictecacihuatl es a quien se le atribuye las primeras historias del dia de muertos, se elaboró un muñexo simulando al personaje mencionado, haciendo uso de papel engrudo para su elaboracion, asi como tambien pintura, las funciones que debia realizar en un inicio fueron pensadas en forma de secuencias, ya que uno de los requerimientos del concurso pedia que fuera interactivo con el publico; para lograr esto el animatronico debia ser contruido con un sensor en la palma de su "mano" el cual tendria encima del sensor una flor, y en ecosistema construido para darle ambientacion a las faldas del animatronico se colocó otro sensor que encima tendria una mariposa, la funcionalidad al retirar la flor era la iluminacion del escenario, poner sus ojos azules y comenzar a cantar mientras mueve la quijada, a esto se le llamaria SECUENCIA 1, para la SECUENCIA 2 al retirar la mariposa, el escenario se apagaba y comenzaba a mover cada una de las articulaciones simulando que se encontraba en molestia o desesperacion, puesto que la mariposa representa una de las almas que estaban bajo su resguardo.


# El animatrónico fue programado en una placa Rasberry con sistema operativo Raspbian y el lenguaje utilizado fue python, las librerias que intervienen en código son las siguientes:

-adafruit_servokit de la cual se importo el paquete especifico: ServoKit (el paquete se buscó dentro del ID Thpnny con el nombre de: "adafruit-circuitpython-servokit) esto para poder usar la placa PCA9685

-gpiozero del cual se importo el paquete LED: esto con la finalidad de controlar las salidas de los pines de la placa GPIO y poder hacer uso del cambio de colores del LED RGB

-time de la cual se importó el paquete sleep: con la finalidad de control de tiempos de espera para poder mover congruentemente los servomotores mediante funciones al igual que los tiempos de encendido del LED

-multiprocessing: esta libreria fue usada para ejecutar eventos simultaneamente y no secuencialmente, esto le daría al animatrónico un aspecto de independencia en cuanto a sus funcionalidades.

-pygame: aun que la funcion principal de la libreria es para videojuegos en python, fue usada por que una de las herramientas que tiene este recurso es la del uso de sonido

-serial: esta libreria fue usada dentro del proyecto para poder establecer comunicación entre la placa Raspberry y la placa Arduino


# Conexiones

- aqui se encuentran algunas imagenes de las conexiones realizadas en las placas

https://drive.google.com/drive/folders/1Sykx4dkL9j5J-jjSWeu4fsEOsgJGC96X


# Código de la placa Arduino UNO

- Funcionalidad del sensor analógico (IR / Sharp)

#define sensorSharp A0
const int led = 13;

void setup(){
  pinMode(led, OUTPUT);
  Serial.begin(9600);
}

void loop(){
  int valor_sharp = analogRead(sensorSharp) * 0.0048828125;
  int distancia = 26*pow(valor_sharp,-1);

  if(distancia >= 6){
    digitalWrite(led, HIGH);
  }

  if(distancia <= 0){
    digitalWrite(led, LOW);
  }

  Serial.println(distancia);
  delay(50);
}


# código de cada extremidad
- Hombro (izquierdo/derecho)
  
from adafruit_servokit import ServoKit #libreria para hacer funcionar adafruit el extensor de servos
from time import sleep #libreria para los tiempos de espera para servo o cualquier otro programa (dispositivo)

#declarando variable para adafruit extension de servo de 16 canales
kit = ServoKit(channels = 16) #declaramos el numero de canales que se necesitan para usar los servos
servo = 16 #en este caso ponemos 4 pero realmente se usaran las posiciones 0,1,2,3
#para este caso usamos el canal 0 -> hombro izquierdo

#los siguientes movimientos se hacen con la finalidad de invertir movimientos por ejemplo cuando bajamos el brazo,
#y no queremos que se quede ahí, realizamos otra funcion para poder volver a subirlo.

def hombro_izquierdo_subir(): #funcion de subir el brazo moviendo el motor del hombro
    kit.servo[1].angle = 90
    sleep(1) 
    kit.servo[1].angle = 0 
    

def hombro_izquierdo_bajar(): #funcion de bajar el brazo moviendo el motor del hombro
    kit.servo[1].angle = 0
    sleep(1)
    kit.servo[1].angle = 90
    

- Codo (Izquierdo / Derecho )

from adafruit_servokit import ServoKit #libreria para hacer funcionar adafruit el extensor de servos
from time import sleep #libreria para los tiempos de espera para servo o cualquier otro programa (dispositivo)

#declarando variable para adafruit extension de servo de 16 canales
kit = ServoKit(channels = 16) #declaramos el numero de canales que se necesitan para usar los servos
servo = 16 #en este caso ponemos 4 pero realmente se usaran las posiciones 0,1,2,3
#para este caso usamos el canal 2 -> codo derecho

#los siguientes movimientos se hacen con la finalidad de invertir movimientos por ejemplo cuando bajamos el brazo,
#y no queremos que se quede ahí, realizamos otra funcion para poder volver a subirlo.

def codo_derecho_subir(): #programación del codo izquierdo para subir
    kit.servo[4].angle = 67 #sube a un ángulo de 67 posteriormente lo dormimos 1 segundo
    sleep(1)
    kit.servo[4].angle = 0 #baja a un ángulo de 0 despues de la espera del segundo anterior y vuelve a dormir 1 seg.
    #sleep(1)

    
def codo_derecho_bajar(): #programación del codo izquierdo para bajar
    kit.servo[4].angle = 67 #baja a un ángulo de 0 para posteriormente dormir 1 seg.
    sleep(1)
    kit.servo[4].angle = 0 #despues de pasado el segundo de reposo, sube nuevamente a un ángulo de 67 y termina el programa
    #sleep(1)
    

- Cuello

from adafruit_servokit import ServoKit #libreria para hacer funcionar adafruit el extensor de servos
from time import sleep #libreria para los tiempos de espera para servo o cualquier otro programa (dispositivo)

#declarando variable para adafruit extension de servo de 16 canales
kit = ServoKit(channels = 16) #declaramos el numero de canales que se necesitan para usar los servos
servo = 6 #en este caso ponemos 4 pero realmente se usaran las posiciones 0,1,2,3
#para este caso usamos el canal 5 -> cuello

#los siguientes movimientos se hacen con la finalidad de invertir movimientos por ejemplo cuando bajamos el brazo,
#y no queremos que se quede ahí, realizamos otra funcion para poder volver a subirlo.

#servo del cuello 75 grados =  mirar a la derecha
#servo del cuello 120 grados = mira al centro
#servo del cuello 180 gradis = mirar a la izquierda

def cuello_izq(): #movieno el motor del cuello para hacer movimiento hacia la izquierda
    kit.servo[5].angle = 120
    sleep(1)
    kit.servo[5].angle = 75
    sleep(1)
    kit.servo[5].angle = 120
    
def cuello_der(): #moviendo el motor del cuello para hacer movimiento hacia la derecha
    kit.servo[5].angle = 120
    sleep(1)
    kit.servo[5].angle = 180
    sleep(1)
    kit.servo[5].angle = 120
    
def activar_cuello(): #funcion donde juntamos ambos movimientos
    cuello_der()
    cuello_izq()

# secuencias

- Secuencia 1:

import multiprocessing
#import pygame
from adafruit_servokit import ServoKit
from gpiozero import LED
from time import sleep

#importando programas
#import codo_derecho
#import codo_izquierdo
#import hombro_derecho
#import hombro_izquierdo
#import mandibula


#declarando variables de los programas
#cd = codo_derecho
#ci = codo_izquierdo
#hd = hombro_derecho
#hi = hombro_izquierdo
#man = mandibula



#Declarando variables para el led RGB
blue = LED(17)
green = LED(27)
red = LED(22)

#declarando variable para adafruit extension de servo de 16 canales
kit = ServoKit(channels = 16)
servo = 16

def corto():
    kit.servo[4].angle = 0
    sleep(0.15)
    kit.servo[4].angle = 45
    sleep(0.15)
    kit.servo[4].angle = 0
    sleep(0.15 )
               
def mediano():
    kit.servo[4].angle = 0
    sleep(0.3)
    kit.servo[4].angle = 45
    sleep(0.3)
    kit.servo[4].angle = 0
    sleep(0.3)
        
def largo():
    kit.servo[4].angle = 0
    sleep(0.4)
    kit.servo[4].angle = 45
    sleep(0.4)
    kit.servo[4].angle = 0
    sleep(0.4)

def canta():
    i = 0
    while i != 1:
        blue.on()
        corto()
        largo()
        corto()
        corto()
        corto()
        sleep(0.5)
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        sleep(0.5)
        corto()
        mediano()
        corto()
        mediano()
        largo()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        sleep(0.5)
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        mediano()
        corto()
        mediano()
        mediano()
        largo()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        mediano()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        corto()
        mediano()
        corto()
        mediano()
        largo()
        corto()
        corto()
        corto()
        corto()
        corto()
        blue.off()
        i += 1

##################Activar Cuello#####################
#servo del cuello 75 grados =  mirar a la derecha
#servo del cuello 120 grados = mira al centro
#servo del cuello 180 gradis = mirar a la izquierda

##############Funcion Codo Derecho#################
    
def codo_derecho_subir(): #programación del codo izquierdo para subir
    kit.servo[2].angle = 67 #sube a un ángulo de 67 posteriormente lo dormimos 1 segundo
    sleep(5)
    kit.servo[2].angle = 20 #baja a un ángulo de 0 despues de la espera del segundo anterior y vuelve a dormir 1 seg.
    #sleep(1)
   
def codo_derecho_bajar(): #programación del codo izquierdo para bajar
    kit.servo[2].angle = 67 #baja a un ángulo de 0 para posteriormente dormir 1 seg.
    sleep(5)
    kit.servo[2].angle = 20 #despues de pasado el segundo de reposo, sube nuevamente a un ángulo de 67 y termina el programa

def activar_codo_derecho():
    codo_derecho_subir()
    codo_derecho_bajar()
    sleep(10)
    codo_derecho_subir()
    sleep(5)
    codo_derecho_bajar()
    sleep(10)
    codo_derecho_subir()
    codo_derecho_bajar()
    
#############Funcion Hombro Derecho###########
    
def hombro_derecho_subir():
    kit.servo[3].angle = 0
    sleep(3)
    kit.servo[3].angle = 90

def hombro_derecho_bajar():
    kit.servo[3].angle = 90
    sleep(3)
    kit.servo[3].angle = 0
    
def activar_hombro_derecho():
    hombro_derecho_subir()
    hombro_derecho_bajar()
    sleep(10)
    hombro_derecho_subir()
    sleep(5)
    hombro_derecho_bajar()
    sleep(10)
    hombro_derecho_subir()
    hombro_derecho_bajar()
    
##########Funcion codo Izquierdo##############

def codo_izquierdo_subir(): #programación del codo izquierdo para subir
    kit.servo[1].angle = 140 #sube a un ángulo de 67 posteriormente lo dormimos 1 segundo
    sleep(3)
    kit.servo[1].angle = 0 #baja a un ángulo de 0 despues de la espera del segundo anterior y vuelve a dormir 1 seg.
        
def codo_izquierdo_bajar(): #programación del codo izquierdo para bajar
    kit.servo[1].angle = 0 #baja a un ángulo de 0 para posteriormente dormir 1 seg.
    sleep(3)
    kit.servo[1].angle = 140 #despues de pasado el segundo de reposo, sube nuevamente a un ángulo de 67 y termina el programa

def activar_codo_izquierdo():
    codo_izquierdo_subir()
    codo_izquierdo_bajar()
    sleep(10)
    codo_izquierdo_subir()
    sleep(5)
    codo_izquierdo_bajar()
    sleep(10)
    codo_izquierdo_subir()
    codo_izquierdo_bajar()
    
#################Funcion Hombro Izquierdo###################
def hombro_izquierdo_subir():
    kit.servo[0].angle = 90
    sleep(3)
    kit.servo[0].angle = 0
    

def hombro_izquierdo_bajar():
    kit.servo[0].angle = 0
    sleep(3)
    kit.servo[0].angle = 90
    
def activar_hombro_izquierdo():
    hombro_izquierdo_subir()
    hombro_izquierdo_bajar()
    sleep(10)
    hombro_izquierdo_subir()
    sleep(5)
    hombro_izquierdo_bajar()
    sleep(10)
    hombro_izquierdo_subir()
    hombro_izquierdo_bajar()


################Funciones de Cintura###############
def cintura_izq():
    kit.servo[6].angle = 0
    sleep(1)
    
def cintura_der():
    kit.servo[6].angle = 90
    sleep(1)
    
def activar_cintura():
    cintura_izq()
    cintura_der()
    cintura_izq()
    cintura_der()
    cintura_izq()
    cintura_der()
    
#Declarando Procesos de Secuencia 1    
#proceso1 = multiprocessing.Process(target = musica)
proceso2 = multiprocessing.Process(target = canta)
#proceso3 = multiprocessing.Process(target = activar_cuello)
proceso3 = multiprocessing.Process(target = activar_hombro_derecho)
proceso4 = multiprocessing.Process(target = activar_codo_derecho)
proceso5 = multiprocessing.Process(target = activar_hombro_izquierdo)
proceso6 = multiprocessing.Process(target = activar_codo_izquierdo)
#proceso8 = multiprocessing.Process(target = activar_cintura)

#Inicio de procesos de secuencia 1
while True:
    #proceso1.start()
    #sleep(6)
    proceso2.start()
    proceso3.start()
    proceso4.start()
    proceso5.start()
    proceso6.start()

    proceso2.join()
    proceso3.join()
    proceso4.join()
    proceso5.join()
    proceso6.join()
    

- Secuencia 2:

import serial
import pygame
from time import sleep
from adafruit_servokit import ServoKit
from time import sleep
from gpiozero import LED

kit=ServoKit(channels = 16)
servo = 16

#Declarando variables para el led RGB
blue = LED(17)
green = LED(27)
red = LED(22)

#nombre del dispositivo serial : dmesg | grep -v disconnect | grep -Eo "tty(ACM|USB)." | tail -l
#ttyACM0 para este caso, pero puede cambiar a ttyACM1
#es el identificador del puerto serial de Arduino
ser = serial.Serial('/dev/ttyACM0',9600)
#es para hacer la transmision de datos de entrada
ser.flushInput()

#variables declaradas para el audio
#inicializamos el pygame
pygame.init()

#inicializamos el mixer
#pygame.mixer.init()
#traemos el archivo de audio desde la ruta
#archivo_audio = "/home/admin/Prueba_servos/Coco/Risa.mp3"
#cargando el archivo a mixer music
#pygame.mixer.music.load(archivo_audio)

def accion_1():
    #mandamos un mensaje con la finalidad de saber si esta funcionando
    print("abierta")
    blue.on()
    red.off()
    #aperturamos el servo 2 al angulo 180
    kit.servo[7].angle = 180 
    
    #aperturamos el servo 3 al angulo 0 asi con los demás
    kit.servo[3].angle = 0
    #kit.servo[4].angle = 180                                                        
    kit.servo[0].angle = 0
    kit.servo[1].angle = 90

    
    #inicializamos el mixer dentro de la comparacion
    pygame.mixer.init()
    #traemos el archivo de audio desde la ruta
    archivo_audio = "/home/admin/Prueba_servos/Coco/Risa.mp3"
    #cargando el archivo a mixer music
    pygame.mixer.music.load(archivo_audio)
    #reproducimos el archivo cargado
    pygame.mixer.music.play()


def accion_2():
    red.on()
    blue.off()
    kit.servo[3].angle = 180
    kit.servo[4].angle = 0
    kit.servo[0].angle = 180
    kit.servo[1].angle = 90
    kit.servo[7].angle = 0
    sleep(0.1)
    kit.servo[7].angle = 180
    sleep(0.1)


#ciclo  para lecturas de Arduino a Raspberry
while True:
    
    try:
        #leemos cada linea de bytes  mediante el comando serial
        lineBytes = ser.readline()
        #convertimos el tipo de bytes y decodificamos al tipo utf-8
        line = lineBytes.decode('utf-8').strip()
        print(line)

        #comparamos si lo que recibimos de la lectura es = al string 13
        if(line == '6' or line == '8'):
            accion_1()
            
            
        else:
            accion_2()
            
            
            
                    
    except KeyboardInterrupt:
        break

# Extras (códigos de pruebas)

-Prueba de Servomotores:

from adafruit_servokit import ServoKit
from time import sleep
kit=ServoKit(channels = 16)
servo = 16

while True:
    a = input("enter: ")
    kit.servo[2].angle = int(a)
    print(a)

- Prueba de Sensores en la placa Arduino comunicando estado a Raspberry:

import serial
import time

#nombre del dispositivo serial : dmesg | grep -v disconnect | grep -Eo "tty(ACM|USB)." | tail -l
ser=serial.Serial('/dev/ttyACM1',9600)
ser.flushInput()


while True:
    try:
        lineBytes = ser.readline()
        line = lineBytes.decode('utf-8').strip()
        
        if int(line) > 13:
            print("flor")
            
        else:
            print("enojo")
            
        
    except KeyboardInterrupt:
        break

# Opiniones
Quizas muchas personas desconoscan esto y aun que está al final no quiere decir que sea menos importante, el motivo por el que fue usada la placa arduino con los sensores, es por que la placa arduino ya trae un convertidor de señal analógica a digital, y la placa raspberry no, por lo que si se queria que funcionaran los sensores era necesario: o tener un convertidor analógico a digital o bien una placa arduino con entradas de señal analógica que normalmente se encuentra en las placas de arduino uno.
El código puede ser optimizado ciertamente pero como se requeria en muy poco tiempo no habia oportunidad de hacerlo, se comentó lo mas posible y espero que le sirva a alguien más.
