//Actividad 3. Cálculo de RFC 
Autor: Araceli Quiñonez

#include <iostream>   // Librería para entrada y salida estándar
#include <string>     // Librería para manejar cadenas de texto
#include <cctype>     // Librería para funciones de manipulación de caracteres
#include <cstdlib>    // Librería para usar system("pause")

// Función que verifica si un carácter es una vocal mayúscula
bool esVocal(char letra) {
    letra = std::toupper(letra);  // Convierte la letra a mayúscula para comparación
    return letra == 'A' || letra == 'E' || letra == 'I' || letra == 'O' || letra == 'U';
}

// Función que obtiene la primera vocal interna del apellido paterno, o 'X' si no hay vocal
char obtenerPrimeraVocalInterna(const std::string& apellidoPaterno) {
    for (size_t indice = 1; indice < apellidoPaterno.length(); ++indice) {  // Recorre el apellido desde la segunda letra
        if (esVocal(apellidoPaterno[indice]))  // Si encuentra una vocal
            return apellidoPaterno[indice];    // La devuelve
    }
    return 'X';  // Si no encuentra vocal, devuelve 'X'
}

// Función que extrae el año, mes y día de la fecha de nacimiento en formato YYMMDD
std::string extraerFechaFormateada(const std::string& fechaNacimiento) {
    if (fechaNacimiento.length() != 10 || fechaNacimiento[4] != '-' || fechaNacimiento[7] != '-') {
        std::cerr << "Error: Formato de fecha incorrecto." << std::endl;  // Verifica si la fecha tiene el formato correcto
        return "";
    }

    std::string anio = fechaNacimiento.substr(2, 2);  // Extrae los dos últimos dígitos del año
    std::string mes = fechaNacimiento.substr(5, 2);   // Extrae el mes
    std::string dia = fechaNacimiento.substr(8, 2);   // Extrae el día

    return anio + mes + dia;  // Retorna la fecha formateada como YYMMDD
}

// Función que calcula el RFC utilizando nombre, apellidos y fecha de nacimiento
std::string calcularRFC(const std::string& nombre, const std::string& apellidoPaterno, const std::string& apellidoMaterno,
    const std::string& fechaNacimiento) {
    std::string rfc;

    rfc += apellidoPaterno[0];                                // Primera letra del apellido paterno
    rfc += obtenerPrimeraVocalInterna(apellidoPaterno);       // Primera vocal interna del apellido paterno
    rfc += !apellidoMaterno.empty() ? apellidoMaterno[0] : 'X';  // Primera letra del apellido materno (o 'X' si no hay)
    rfc += nombre[0];                                         // Primera letra del nombre
    rfc += extraerFechaFormateada(fechaNacimiento);           // Fecha de nacimiento en formato YYMMDD

    return rfc;  // Retorna el RFC calculado
}

// Función que verifica si el RFC generado forma palabras inapropiadas y la modifica si es necesario
std::string validarRFC(std::string rfc) {
                                                      // Lista de palabras inapropiadas
    std::string palabrasInapropiadas[] = { "BUEI", "GUEY", "CACA", "CAGA", "CAKA", "COGE", "COJE", "COJO", "FETO", "NACO", "NACA",
 "PELO", "PALO", "SEXY", "SEXO", "JOTO", "KACO", "KAGO", "CAGO", "KOJO", "KOGE", "KOJE", "KULO", "MAMO", "MEAS", "MION", "PENE", "WUEY",
 "MAME", "MAMA", "PAPA", "MAMI", "PAPI" "NINI", "MULA", "PEDO", "PEDA", "PUTA", "PUTO", "QULO", "RUIN", "PUTI", "CULO", "TETA", "JAJA"};

    // Verifica si el RFC generado contiene una palabra inapropiada
    for (const auto& palabra : palabrasInapropiadas) {
        if (rfc.substr(0, 4) == palabra) {
            rfc[3] = 'X';  // Reemplaza la última letra del primer bloque por 'X'
            break;
        }
    }
    return rfc;
}

int main() {
    // Variables para almacenar los datos del usuario
    std::string nombre, apellidoPaterno, apellidoMaterno, fechaNacimiento;
    //Texto inicial
    std::cout << "CALCULO DEL RFC \n\nPor favor introduce los siguientes datos en MAYUSCULAS:\n\n";

    // Solicitar al usuario que ingrese su nombre
    std::cout << "Introduce tu nombre: ";
    std::getline(std::cin, nombre);

    // Solicitar al usuario que ingrese su apellido paterno
    std::cout << "Introduce tu apellido paterno: ";
    std::getline(std::cin, apellidoPaterno);

    // Solicitar al usuario que ingrese su apellido materno (o dejar vacío)
    std::cout << "Introduce tu apellido materno: (Si no tiene, pulse enter) ";
    std::getline(std::cin, apellidoMaterno);

    // Solicitar al usuario que ingrese su fecha de nacimiento en formato YYYY-MM-DD
    std::cout << "Introduce tu fecha de nacimiento: (YYYY-MM-DD) ";
    std::getline(std::cin, fechaNacimiento);

    // Calcular el RFC usando los datos ingresados
    std::string rfc = calcularRFC(nombre, apellidoPaterno, apellidoMaterno, fechaNacimiento);

    // Verificar el RFC y hacer modificaciones si es necesario
    rfc = validarRFC(rfc);

    // Mostrar el RFC calculado en la pantalla
    if (!rfc.empty()) {
        std::cout << "Tu RFC sin homoclave es: " << rfc << std::endl <<std::endl;
    }

    // Pausar la consola para que no se cierre inmediatamente
    system("pause");

    return 0;  // Finaliza el programa
