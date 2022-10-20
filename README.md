# Reservaciones
Reservaciones
# Se importa modulo para trabajar con el sistema operativo
import os
# Se importa módulo para trabajar con archivos csv
import csv
# Se importa módulo para trabajar con expresiones regulares
import re
# Se importa módulo para trabajar con datos tipo datetime
import datetime
from operator import attrgetter, truediv





captura = ""
LimpiarPantalla = lambda: os.system('cls')


global nicknameC
global nicknameS
global nombreC
global nombreS
global cupo
global fechrev
global turno

# Se crea la clase Cliente
class Cliente:
    def __init__(self, NICKNAME, NOMBRE):
        # NICKNAME: clave unica del cliente
        # NOMBRE: Nombre completo del contacto

        self.NICKNAME = NICKNAME
        self.NOMBRE = NOMBRE

class Turno:
    def __init__(self, NOMBRE):
        # NICKNAME: clave unica del cliente
        # NOMBRE: Nombre completo del contacto
        self.NOMBRE = NOMBRE

# Se crea la clase Sala
class Sala:
    def __init__(self, NICKNAME, NOMBRE, CUPO):
        # NICKNAME: clave unica de la Sala
        # NOMBRE: Nombre completo de la sala

        self.NICKNAME = NICKNAME
        self.NOMBRE = NOMBRE
        self.CUPO = CUPO

# Se crea la clase reservacion
class Reservacion:
    def __init__(self, NICKNAME, NOMBRE, CLIENTE, SALA, FECHARESERVA, TURNO):
        # NICKNAME: clave unica de la sala
        # NOMBRE: Nombre completo de la sala
        # CUPO: cupo de la sala
        # FECHARESERVA: Fecha de reserva
        # TURNO: Turno de la sala
        self.NICKNAME = NICKNAME
        self.NOMBRE = NOMBRE
        self.CLIENTE = CLIENTE
        self.SALA = SALA
        self.FECHARESERVA = FECHARESERVA
        self.TURNO = TURNO

# Se crea la clase reporte
class Reporte:
    def __init__(self, NICKNAME, NOMBRE, CLIENTE, SALA, FECHARESERVA, TURNO):
        # NICKNAME: clave unica de la sala
        # NOMBRE: Nombre completo de la sala
        # CUPO: cupo de la sala
        # FECHARESERVA: Fecha de reserva
        # TURNO: Turno de la sala
        self.NICKNAME = NICKNAME
        self.NOMBRE = NOMBRE
        self.CLIENTE = CLIENTE
        self.SALA = SALA
        self.FECHARESERVA = FECHARESERVA
        self.TURNO = TURNO

Turnos = []
objeto_temporal = Turno('MATUTINO')
Turnos.append(objeto_temporal)
objeto_temporal = Turno('VESPERTINO')
Turnos.append(objeto_temporal)
objeto_temporal = Turno('NOCTURNO')
Turnos.append(objeto_temporal)


#función para verificar si existen los archivos csv
def CrearArchivo(Archivo):
    ruta = os.path.abspath(os.getcwd())
    archivo_trabajo=ruta+Archivo
    # Se determina si el archivo de trabajo ya existe
    if os.path.exists(archivo_trabajo):
        # Si el archivo existe, entonces verifica si hay respaldo y lo borra
        print('el archivo existe podemos continuar')
    else:
        f = open(archivo_trabajo,"w+")
        print('se a creado el archivo' + Archivo)

#Se crean los Csv o se valida que existan
CrearArchivo('\\Clientes.csv')
CrearArchivo('\\Salas.csv')
CrearArchivo('\\Reservaciones.csv')
CrearArchivo('\\Reportes.csv')



with open('Clientes.csv') as archivo_csv:
    lector_csv = csv.reader(archivo_csv, delimiter='|')
    contador_lineas = 0
    # Creando una lista vacía.
    Clientes = []
    # Lectura secuencial.
    for linea_datos in lector_csv:
        if contador_lineas == 0:
            # Si es la primer línea, muestro los nombres de campo y no guardo nada
            # en la lista.
            print(f'Los nombres de columna son {", ".join(linea_datos)}')
        else:
            # Si son datos (línea uno y posteriores)...
            # Genero una instancia de la clase Incidente, y le proporciono al constructor
            # los valores leidos del archivo.
            objeto_temporal = Cliente(linea_datos[0],linea_datos[1])
            Clientes.append(objeto_temporal)

        # Se incrementa el número de líneas, pase lo que pase.
        contador_lineas += 1

with open('Salas.csv') as archivo_csv:
    lector_csv = csv.reader(archivo_csv, delimiter='|')
    contador_lineas = 0
    # Creando una lista vacía.
    Salas = []
    # Lectura secuencial.
    for linea_datos in lector_csv:
        if contador_lineas == 0:
            # Si es la primer línea, muestro los nombres de campo y no guardo nada
            # en la lista.
            print(f'Los nombres de columna son {", ".join(linea_datos)}')
        else:
            # Si son datos (línea uno y posteriores)...
            # Genero una instancia de la clase Incidente, y le proporciono al constructor
            # los valores leidos del archivo.
            objeto_temporal = Sala(linea_datos[0],linea_datos[1],linea_datos[2])
            Salas.append(objeto_temporal)

        # Se incrementa el número de líneas, pase lo que pase.
        contador_lineas += 1

with open('Reservaciones.csv') as archivo_csv:
    lector_csv = csv.reader(archivo_csv, delimiter='|')
    contador_lineas = 0
    # Creando una lista vacía.
    Reservaciones = []
    # Lectura secuencial.
    for linea_datos in lector_csv:
        if contador_lineas == 0:
            # Si es la primer línea, muestro los nombres de campo y no guardo nada
            # en la lista.
            print(f'Los nombres de columna son {", ".join(linea_datos)}')
        else:
            # Si son datos (línea uno y posteriores)...
            # Genero una instancia de la clase Incidente, y le proporciono al constructor
            # los valores leidos del archivo.
            objeto_temporal = Reservacion(linea_datos[0],linea_datos[1],linea_datos[2],linea_datos[3], linea_datos[4],linea_datos[5])
            Reservaciones.append(objeto_temporal)

        # Se incrementa el número de líneas, pase lo que pase.
        contador_lineas += 1

Reportes = []
# Función para convertir cadena a datetime
def cadenafecha(_dtDato):
    return datetime.datetime(int(_dtDato[0:4]), int(_dtDato[5:7]), int(_dtDato[-2:]))


# Función para validar la fecha
def preguntaFecha(_formato,_pregunta="Datos: "):
    # Se especifica que "_captura" es global
    global _captura
    while True:
      _dato = input(_pregunta)
      valido = re.search(_formato,_dato)
      if (valido):
        try:
            FechaTexto = _dato
            FechaFCH = datetime.datetime.strptime(FechaTexto, '%Y %m %d')
            
            #FechaAct2 se guarda la fecha de hoy + 2 dias
            FechaAct2 = (datetime.timedelta(days=2) + datetime.datetime.now()) - (datetime.datetime.now())

            #FechaFut es la fecha que se inserto - la fecha actual
            FechaFut = FechaFCH - (datetime.datetime.now())

            if(FechaAct2 < FechaFut):
                _captura = _dato
                return(1)
            
        except:
            print('La fecha no es valida, insterte una fecha valida, debe ser con 2 dias de anticipacion')
        break
            
      else:
        print("Dato no válido. Intente de nuevo")


def OrdenarCsv (Lista):
    Lista.sort(key=attrgetter("NICKNAME"),reverse=False)

# Función para validar los datos
def pregunta(_formato,_pregunta="Datos: "):
    # Se especifica que "_captura" es global
    global _captura
    while True:
      _dato = input(_pregunta)
      valido = re.search(_formato,_dato)
      if (valido):
        _captura = _dato
        break
      else:
        print("Dato no válido. Intente de nuevo")

# Función para validar los datos
def preguntaTurno(_formato,_pregunta="Datos: "):
    # Se especifica que "_captura" es global
    global _captura
    while True:
      _dato = input(_pregunta)
      valido = re.search(_formato,_dato)
      if (valido):
        if _dato == 'MATUTINO':
            _captura = _dato
            break
        elif _dato == 'VESPERTINO':
            _captura = _dato
            break
        elif _dato == 'NOCTURNO':
            _captura = _dato
            break
        else:
            print("Inserte un turno valido")
        
        
      else:
        print("Dato no válido. Intente de nuevo")

# Función para validar los datos de cliente existente
def preguntaCliente(_formato,_pregunta="Datos: "):
    # Se especifica que "_captura" es global
    global _captura
    while True:
      _dato = input(_pregunta)
      valido = re.search(_formato,_dato)
      if (valido):
        contador =- 1
        indice_retorno =- 1
        for Cliente in Clientes:
            contador += 1
            if (Cliente.NOMBRE == _dato):
                indice_retorno = contador

        if indice_retorno==-1:
            print("No se encontró el cliente, insterte uno valido")
            

        else:
            print('El cliente es: ')
            print(Clientes[indice_retorno].NICKNAME)
            print(Clientes[indice_retorno].NOMBRE)
            _captura = _dato
            return(1)

      else:
        print("Dato no válido. Intente de nuevo")

# Función para validar los datos de cliente existente
def preguntaSala(_formato,_pregunta="Datos: "):
    # Se especifica que "_captura" es global
    global _captura
    while True:
      _dato = input(_pregunta)
      valido = re.search(_formato,_dato)
      if (valido):
        contador =- 1
        indice_retorno =- 1
        for Sala in Salas:
            contador += 1
            if (Sala.NOMBRE == _dato):
                indice_retorno = contador

        if indice_retorno==-1:
            print("No se encontró la sala, insterte una valida")
            return(0)
        else:
            print('La sala es: ')
            print(Salas[indice_retorno].NICKNAME)
            print(Salas[indice_retorno].NOMBRE)
            _captura = _dato
            return(1)

      else:
        print("Dato no válido. Intente de nuevo")

# Función para validar los datos
def preguntaCupo(_formato,_pregunta="Datos: "):
    # Se especifica que "_captura" es global
    global _captura
    while True:
      _dato = input(_pregunta)
      valido = re.search(_formato,_dato)
      if (valido):
        if int(_dato) > 0 :
            _captura = int(_dato)
            break
        else:
            print('el cupo debe ser mayor a 0')
      else:
        print("Dato no válido. Intente de nuevo")

# Función para captura de datos de los Clientes
def ingresoDatosClientes():
    global nicknameC
    global nombreC
    # Se da el nickname o clave unica
    with open('Clientes.csv') as archivo_csv:
    # Se habilita objeto que permitira leer de manera secuencial el contenido del archivo csv
    # archivo_csv es el puente entre el programa y el archivo
    # lector_csv es el flujo de datos entre el programa y el archivo
        lector_csv = csv.reader(archivo_csv, delimiter='|')
        # Contador de lineas
        contador_lineas = 0

        # Lee secuencialmente el archivo usando el flujo de datos (lector_csv)
        # La linea se coloca en el elemento linea
        # Al trabajar con <<linea_datos>> se trabaja con la linea del archivo
        # for dejará de leer cuando los datos del archivo se hayan terminado
        for linea_datos in lector_csv:
                # Si el contador es mayor a cero: muestra, dato por dato, los datos obtenidos en linea_datos
                if contador_lineas > 0:
                    IDstr = (f'\t{linea_datos[0]}')
                contador_lineas += 1
    try:
        ID = int(IDstr)
        if(ID > (0)):
            nicknameC = ID + 1
    except:
        nicknameC = 1
    
    # Se pide el nombre completo
    pregunta("^[A-Z ]{1,35}$", "Ingrese nombre completo del cliente en mayusculas: ")
    nombreC = _captura

    Clientes.append(Cliente(nicknameC,nombreC,))
    print('')
    print('############################')
    print('Cliente Creado con exito')
    print('############################')

# Función para captura de datos de las Salas
def ingresoDatosSalas():

    # Se da el nickname o clave unica 

    with open('Salas.csv') as archivo_csv:
    # Se habilita objeto que permitira leer de manera secuencial el contenido del archivo csv
    # archivo_csv es el puente entre el programa y el archivo
    # lector_csv es el flujo de datos entre el programa y el archivo
        lector_csv = csv.reader(archivo_csv, delimiter='|')
        # Contador de lineas
        contador_lineas = 0

        # Lee secuencialmente el archivo usando el flujo de datos (lector_csv)
        # La linea se coloca en el elemento linea
        # Al trabajar con <<linea_datos>> se trabaja con la linea del archivo
        # for dejará de leer cuando los datos del archivo se hayan terminado
        for linea_datos in lector_csv:
                # Si el contador es mayor a cero: muestra, dato por dato, los datos obtenidos en linea_datos
                if contador_lineas > 0:
                    IDstr = (f'\t{linea_datos[0]}')
                contador_lineas += 1

    try:
        ID = int(IDstr)
        if(ID > (0)):
            nicknameS = ID + 1
    except:
        nicknameS = 1
    
    # Se pide el nombre completo
    pregunta("^[A-Z ]{1,35}$", "Ingrese nombre completo de la sala en mayusculas: ")
    nombre = _captura
    #pregunta("^([0-9]){1,7}$", "Ingrese cupo: ")
    preguntaCupo("^[0-9]+(\.[0-9]{1,2})?$", "Ingrese cupo mayor a 0: ")
    cupo = _captura

    Salas.append(Sala(nicknameS,nombre,cupo))
    print('')
    print('############################')
    print('Sala Creada con exito')
    print('############################')

#Función para captura de datos de las reservaciones
def ingresoDatosReservaciones():

    # Se da el nickname o clave unica 
    with open('Reservaciones.csv') as archivo_csv:
    # Se habilita objeto que permitira leer de manera secuencial el contenido del archivo csv
    # archivo_csv es el puente entre el programa y el archivo
    # lector_csv es el flujo de datos entre el programa y el archivo
        lector_csv = csv.reader(archivo_csv, delimiter='|')
        # Contador de lineas
        contador_lineas = 0

        # Lee secuencialmente el archivo usando el flujo de datos (lector_csv)
        # La linea se coloca en el elemento linea
        # Al trabajar con <<linea_datos>> se trabaja con la linea del archivo
        # for dejará de leer cuando los datos del archivo se hayan terminado
        for linea_datos in lector_csv:
                # Si el contador es mayor a cero: muestra, dato por dato, los datos obtenidos en linea_datos
                if contador_lineas > 0:
                    IDstr = (f'\t{linea_datos[0]}')
                contador_lineas += 1

    try:
        ID = int(IDstr)
        if(ID > (0)):
            nicknameR = ID + 1
    except:
        nicknameR = 1

    pregunta("^[A-Z ]{1,35}$", "Ingrese nombre del evento completo en mayusculas: ")
    nombre = _captura
    while True:
        if preguntaCliente("^[A-Z ]{1,35}$", "Ingrese nombre de cliente completo en mayusculas: ") == 1:
            cliente = _captura
            break
        else:
            print('el cliente no se encontró')
    while True:
        if preguntaSala("^[A-Z ]{1,35}$", "Ingrese nombre de la sala completo en mayusculas: ") == 1:
            sala = _captura
            break
        else:
            print('La SALA no se encontró')        
    

    while True:
        if preguntaFecha("^[0-9]{4} [0-9]{2} [0-9]{2}", "Ingrese fecha de reservacion (YYYY MM DD): ") == 1:
            fechaRev = _captura
            break
        else:
            print('La Fecha no es falida')
    

    preguntaTurno("^[A-Z ]{1,35}$", "Ingrese el turno en mayusculas MATUTINO, VESPERTINO O NOCTURNO: ")
    turno = _captura

    contador_fecha =- 1
    indice_retorno_fecha =- 1
    for Reserva in Reservaciones:
        contador_fecha += 1
        if ((Reserva.FECHARESERVA == fechaRev) and (Reserva.SALA == sala) and (Reserva.TURNO == turno)):
            indice_retorno_fecha = contador_fecha
            break
    
    if indice_retorno_fecha ==-1:
        Reservaciones.append(Reservacion(nicknameR,nombre,cliente,sala,fechaRev,turno))
        print('')
        print('############################')
        print('Reservacion Creada con exito')
        print('############################')

    else:
        print()
        print('El turno en esa fecha esta reservada, vaya a verificar la disponibilidad en "dispnibilidad de sala"')
        print( 'El evento es ')
        print(Reservaciones[indice_retorno_fecha].NICKNAME)
        print(Reservaciones[indice_retorno_fecha].NOMBRE)
        print(Reservaciones[indice_retorno_fecha].CLIENTE)
        print(Reservaciones[indice_retorno_fecha].SALA)
        print(Reservaciones[indice_retorno_fecha].FECHARESERVA)
        print(Reservaciones[indice_retorno_fecha].TURNO)

# Función para validar la existencia del NICKNAME buscado de una sala
def buscarCliente(NicknameBuscar):
    contador =- 1
    indice_retorno =- 1
    for Cliente in Clientes:
        contador += 1
        if (Cliente.NICKNAME == NicknameBuscar):
            indice_retorno = contador
            break
    return indice_retorno

# Función para validar la existencia del NOMBRE buscado de una reservacion
def buscarReservacion(NombreBuscar):
    contador =- 1
    indice_retorno =- 1
    for Reserva in Reservaciones:
        contador += 1
        if (Reserva.NOMBRE == NombreBuscar):
            indice_retorno = contador
            break
    return indice_retorno

#Función que verifica si hay reservaciones en cierta fecha
def buscarReporteReservacion(FechaBuscar):
    contador =- 1
    indice_retorno =- 1
    print('##############################################################')
    print('####    Reporte de reservaciones para el dia ' + FechaBuscar + '  ####')
    print('##############################################################')
    print ("{:<10} {:<15} {:<10} {:<10} {:<15} {:<10}".format('NICKNAME','NOMBRE','CLIENTE','SALA','FECHA RESERVA', 'TURNO'))
    print('')

    for Reserva in Reservaciones:
        contador += 1
        if (Reserva.FECHARESERVA == FechaBuscar):
            indice_retorno = contador
            nicknamet = Reservaciones[indice_retorno].NICKNAME
            nombret = Reservaciones[indice_retorno].NOMBRE
            clientet = Reservaciones[indice_retorno].CLIENTE
            salat = Reservaciones[indice_retorno].SALA
            fechat = Reservaciones[indice_retorno].FECHARESERVA
            turnot = Reservaciones[indice_retorno].TURNO

            objeto_temporal = Reporte(nicknamet,nombret,clientet,salat,fechat,turnot)
            Reportes.append(objeto_temporal)
            
            print ("{:<10} {:<15} {:<10} {:<10} {:<15} {:<10}".format(Reservaciones[indice_retorno].NICKNAME,Reservaciones[indice_retorno].NOMBRE,Reservaciones[indice_retorno].CLIENTE
            ,Reservaciones[indice_retorno].SALA,Reservaciones[indice_retorno].FECHARESERVA, Reservaciones[indice_retorno].TURNO))
            

    if indice_retorno==-1:
        print('no hay reservaciones en esa fecha')
    else:    
        print('')
        while (True):

            print("Desea exportar este reporte a excel? ")
            print(" ")
            print("[1] Si.")
            print("[0] No.")
            opcion_elegida = input("¿Qué deseas hacer?  > ")
            if RegEx(opcion_elegida,"^[10]{1}$"):
                if opcion_elegida == "0":
                    print("Volviendo al menú principal...")
                    principal()
                    quit()
                

                if opcion_elegida == "1":
                    FinalizaReportes()
                    FinalizarClientes()
                    FinalizaSalas()
                    FinalizaReservaciones()
                    quit()

                input("Pulse enter para continuar...")
                break
            else:
                print("Respuesta no válida.")
                input("Pulse enter para contunuar...")

                    


    return indice_retorno



def buscarReporteDisponibilidad(FechaBuscar):
    contador =- 1
    contador2 =-1
    contador3 =-1
    indice_retorno =- 1
    print('##############################################################')
    print('####Reporte de reservaciones disponibles para el dia ' + FechaBuscar + '####')
    print('##############################################################')
    print("{:<10} {:<15} {:<10}".format('NICKNAME','SALA', 'TURNO'))
    print('')



                         
    for Reserva in Reservaciones:
            contador += 1
            if (Reserva.FECHARESERVA == FechaBuscar):
                indice_retorno = contador
                for turno in Turnos:
                    if(Reservaciones[indice_retorno].TURNO != turno.NOMBRE):
                        var=0
                        for sala in Salas:
                            if((Reservaciones[indice_retorno].SALA != sala.NOMBRE) and (Reservaciones[indice_retorno].TURNO != turno.NOMBRE)):
                                var=0
                            else:
                                print("{:<10} {:<15} {:<10}".format(sala.NICKNAME,sala.NOMBRE,turno.NOMBRE))
                            contador2 += 1

                    else:
                        
                        for sala in Salas:

                            if(Reservaciones[indice_retorno].SALA  != sala.NOMBRE):
                                var=0
                            else:

                                print("{:<10} {:<15} {:<10}".format(sala.NICKNAME,sala.NOMBRE,turno.NOMBRE))
                            contador2 += 1
                    contador3 +=1
                        

            
                         
                            



    


    #imprime todas las salas con todos los turnos si es que no hay ninguna reserva
    if indice_retorno==-1:
        for sala in Salas:
            for turno in Turnos:
                print("{:<10} {:<15} {:<10}".format(sala.NICKNAME,sala.NOMBRE,turno))
                
            
    

    


    
                    


    return indice_retorno

# Función para validador de opciones
def RegEx(_txt,_regex):
    coincidencia = re.match(_regex, _txt)
    return bool(coincidencia)

#---------------------------------------------------------
def FinalizarClientes():    
    # Se guarda en la variable la ruta absoluta, del directorio actual de trabajo (cwd)
    ruta = os.path.abspath(os.getcwd())
    archivo_trabajo=ruta+"\\Clientes.csv"
    archivo_respaldo=ruta+"\\Clientes.bak"

    # Se determina si el archivo de trabajo ya existe
    if os.path.exists(archivo_trabajo):
        # Si el archivo existe, entonces verifica si hay respaldo y lo borra
        if os.path.exists(archivo_respaldo):
            os.remove(archivo_respaldo)

        # Establece el achivo de datos, como respaldo
        os.rename(archivo_trabajo,archivo_respaldo)

    # Genera el archivo CSV
    f = open(archivo_trabajo,"w+")
    # Se escriben los encabezados del archivo csv
    f.write("NICKNAME|NOMBRE\n")
    # Se escribe en el csv, a partir de la lista de objetos
    for elemento in Clientes:
        f.write(f'{elemento.NICKNAME}|{elemento.NOMBRE}\n')

    # Se cierra el archivo csv
    f.close()
    print("Elementos de clientes guardados")

def FinalizaSalas():
    # Se guarda en la variable la ruta absoluta, del directorio actual de trabajo (cwd)
    ruta = os.path.abspath(os.getcwd())
    archivo_trabajo=ruta+"\\Salas.csv"
    archivo_respaldo=ruta+"\\Salas.bak"

    # Se determina si el archivo de trabajo ya existe
    if os.path.exists(archivo_trabajo):
        # Si el archivo existe, entonces verifica si hay respaldo y lo borra
        if os.path.exists(archivo_respaldo):
            os.remove(archivo_respaldo)

        # Establece el achivo de datos, como respaldo
        os.rename(archivo_trabajo,archivo_respaldo)

    # Genera el archivo CSV
    f = open(archivo_trabajo,"w+")
    # Se escriben los encabezados del archivo csv
    f.write("NICKNAME|NOMBRE|CUPO\n")
    # Se escribe en el csv, a partir de la lista de objetos
    for elemento in Salas:
        f.write(f'{elemento.NICKNAME}|{elemento.NOMBRE}|{elemento.CUPO}\n')

    # Se cierra el archivo csv
    f.close()
    print("Elementos de salas guardados")

def FinalizaReservaciones():
    # Se guarda en la variable la ruta absoluta, del directorio actual de trabajo (cwd)
    ruta = os.path.abspath(os.getcwd())
    archivo_trabajo=ruta+"\\Reservaciones.csv"
    archivo_respaldo=ruta+"\\Reservaciones.bak"

    # Se determina si el archivo de trabajo ya existe
    if os.path.exists(archivo_trabajo):
        # Si el archivo existe, entonces verifica si hay respaldo y lo borra
        if os.path.exists(archivo_respaldo):
            os.remove(archivo_respaldo)

        # Establece el achivo de datos, como respaldo
        os.rename(archivo_trabajo,archivo_respaldo)

    # Genera el archivo CSV
    f = open(archivo_trabajo,"w+")
    # Se escriben los encabezados del archivo csv
    f.write("NICKNAME|NOMBRE|CLIENTE|SALA|FECHARESERVA|TURNO\n")
    # Se escribe en el csv, a partir de la lista de objetos
    for elemento in Reservaciones:
        f.write(f'{elemento.NICKNAME}|{elemento.NOMBRE}|{elemento.CLIENTE}|{elemento.SALA}|{elemento.FECHARESERVA}|{elemento.TURNO}\n')

    # Se cierra el archivo csv
    f.close()
    print("Elementos de reservaciones Guardados")

def FinalizaReportes():
    # Se guarda en la variable la ruta absoluta, del directorio actual de trabajo (cwd)
    ruta = os.path.abspath(os.getcwd())
    archivo_trabajo=ruta+"\\Reportes.csv"
    archivo_respaldo=ruta+"\\Reportes.bak"

    # Se determina si el archivo de trabajo ya existe
    if os.path.exists(archivo_trabajo):
        # Si el archivo existe, entonces verifica si hay respaldo y lo borra
        if os.path.exists(archivo_respaldo):
            os.remove(archivo_respaldo)

        # Establece el achivo de datos, como respaldo
        os.rename(archivo_trabajo,archivo_respaldo)

    # Genera el archivo CSV
    f = open(archivo_trabajo,"w+")
    # Se escriben los encabezados del archivo csv
    f.write("NICKNAME|NOMBRE|CLIENTE|SALA|FECHARESERVA|TURNO\n")
    # Se escribe en el csv, a partir de la lista de objetos
    for elemento in Reportes:
        f.write(f'{elemento.NICKNAME}|{elemento.NOMBRE}|{elemento.CLIENTE}|{elemento.SALA}|{elemento.FECHARESERVA}|{elemento.TURNO}\n')

    # Se cierra el archivo csv
    f.close()
    print("Elementos de reportes Guardados")

def InicializarReservaciones():
    Reservaciones.sort(key=attrgetter("NICKNAME"),reverse=False)    
    # Se guarda en la variable la ruta absoluta, del directorio actual de trabajo (cwd)
    ruta = os.path.abspath(os.getcwd())
    archivo_trabajo=ruta+"\\Reservaciones.csv"
    archivo_respaldo=ruta+"\\Reservaciones.bak"

    # Se determina si el archivo de trabajo ya existe
    if os.path.exists(archivo_trabajo):
        # Si el archivo existe, entonces verifica si hay respaldo y lo borra
        if os.path.exists(archivo_respaldo):
            os.remove(archivo_respaldo)

        # Establece el achivo de datos, como respaldo
        os.rename(archivo_trabajo,archivo_respaldo)

    # Genera el archivo CSV
    f = open(archivo_trabajo,"w+")
    # Se escriben los encabezados del archivo csv
    f.write("NICKNAME|NOMBRE|CLIENTE|SALA|FECHARESERVA|TURNO\n")
    # Se escribe en el csv, a partir de la lista de objetos
    for elemento in Reservaciones:
        f.write(f'{elemento.NICKNAME}|{elemento.NOMBRE}|{elemento.CLIENTE}|{elemento.SALA}|{elemento.FECHARESERVA}|{elemento.TURNO}\n')

    # Se cierra el archivo csv
    f.close()

# Funcion que ejecuta el menú, se repite hasta que el usuario ingrese "0"
def principal():
    while (True):
        print("LISTA DE OPCIONES")
        print(" ")
        print("[1] Agregar un Cliente.")
        print("[2] Agregar una Sala.")
        print("[3] Menú de reservaciones")
        print("[4] Menú de reportes.")
        print("[5] Mostrar disponibilidad en una fecha.")
        print("[0] Salir.")
        opcion_elegida = input("¿Qué deseas hacer?  > ")
        if RegEx(opcion_elegida,"^[123450]{1}$"):
            if opcion_elegida == "0":
                print("GRACIAS POR UTILIZAR EL PROGRAMA")
                FinalizarClientes()
                FinalizaSalas()
                FinalizaReservaciones()
                break

            if opcion_elegida == "1":
                print("Llamar procedimiento para la acción")
                ingresoDatosClientes()

            if opcion_elegida == "2":
                print("Llamar procedimiento para la acción")
                ingresoDatosSalas()
                

            if opcion_elegida == "3":
                print("Llamar procedimiento para la acción")
                principalReservaciones()
            
            if opcion_elegida == "4":
                print("Llamar procedimiento para la acción")
                principalReportes()

            if opcion_elegida == "5":
                print("Llamar procedimiento para la acción")
                FechaDisp = input('ingresa la fecha a verificar disponibilidad YYYY MM DD ')
                buscarReporteDisponibilidad(FechaDisp)


            input("Pulse enter para continuar...")
        else:
            print("Respuesta no válida.")
            input("Pulse enter para contunuar...")

# Funcion que ejecuta el menú de reservaciones, se repite hasta que el usuario ingrese "0"
def principalReservaciones():
    while (True):
        print("LISTA DE OPCIONES de reservaciones")
        print(" ")
        print("[1] Registrar nueva reservacion.")
        print("[2] Modificar descripcion de una reservacion.")
        print("[0] Regresar al menú principal.")
        opcion_elegida = input("¿Qué deseas hacer?  > ")
        if RegEx(opcion_elegida,"^[12]{1}$"):
            if opcion_elegida == "1":
                print("llamar procedimiento para la acción")
                ingresoDatosReservaciones()
            
            if opcion_elegida == "2":
                print("llamar procedimiento para la acción")
                VariableNombre = input("inserte el nombre de la reservación en mayusculas: ")
                indice_obtenido = buscarReservacion(VariableNombre)
                if indice_obtenido==-1:
                    print("No se encontró la reservacion")
                else:
                    print("La reservacion a modificar será: ")
                    print('')
                    print(Reservaciones[indice_obtenido].NICKNAME)
                    print(Reservaciones[indice_obtenido].NOMBRE)
                    print(Reservaciones[indice_obtenido].CLIENTE)
                    print(Reservaciones[indice_obtenido].SALA)
                    print(Reservaciones[indice_obtenido].FECHARESERVA)
                    print(Reservaciones[indice_obtenido].TURNO)
                    

                    print('')
                    print('')
                    nombre_nuevo = input('Que nombre quiere insertar para modificar? insertar en mayusculas ')


                    Reservaciones.append(Reservacion(Reservaciones[indice_obtenido].NICKNAME,nombre_nuevo,Reservaciones[indice_obtenido].CLIENTE,Reservaciones[indice_obtenido].SALA,Reservaciones[indice_obtenido].FECHARESERVA,Reservaciones[indice_obtenido].TURNO))
                    del Reservaciones[indice_obtenido]
        if opcion_elegida == "0":
                print("Volviendo al menú principal...")
                principal()
                quit()
                        
# Funcion que ejecuta el menú de reportes, se repite hasta que el usuario ingrese "0"
def principalReportes():
    while (True):
        
        print("LISTA DE OPCIONES de reportes")
        print(" ")
        print("[1] mostrar reporte de reservaciones para una fecha.")
        print("[0] Regresar al menú principal.")
        opcion_elegida = input("¿Qué deseas hacer?  > ")
        if RegEx(opcion_elegida,"^[10]{1}$"):
            if opcion_elegida == "0":
                print("Volviendo al menú principal...")
                principal()
                break

            if opcion_elegida == "1":
                print("llamar procedimiento para la acción")
                VariableFecha = input("inserte el fecha YYYY MM DD:  ")
                indice_obtenido = buscarReporteReservacion(VariableFecha)
                if indice_obtenido==-1:
                    print("No hay reservaciones en esa fecha")
                else:
                    principal()
                    quit()
                    




            input("Pulse enter para continuar...")
        else:
            print("Respuesta no válida.")
            input("Pulse enter para contunuar...")


#-------------------------------------------------

InicializarReservaciones()
principal()

