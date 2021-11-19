#include <iostream>
#include <fstream>
#include "listasutil.hpp"
#include "listasorde.hpp"
#include "rwstring.hpp"

using namespace std;

struct Registro{
    int id;
    string cliente;
    string producto;
    float valor;

};

const int lcliente = 12;
const int lproducto = 10;



ostream& operator << (ostream &os, const Registro& reg)
{
	os << "id: " << reg.id << " - " << reg.cliente << " - " << reg.producto
	   << " - cant " << reg.valor;
	return os;
}

fstream& operator >> (fstream &fs, Registro& reg)
{
	fs.read(reinterpret_cast<char *>(&reg.id), sizeof(reg.id));
	reg.cliente = readstring(fs, lcliente);
	reg.producto = readstring(fs, lproducto);
	fs.read(reinterpret_cast<char *>(&reg.valor), sizeof(reg.valor));
	return fs;
}



int critclieprod (Registro a, Registro b)
{
    if (a.cliente==b.cliente)
        return a.producto.compare(b.producto);
    else
        return a.cliente > b.cliente ? 1 : -1;
} 



int critid (Registro a, Registro b)
{
	return a.id - b.id;
}


void pedirNumero(int& opcion){
	
	
	cin>>opcion;
	if(opcion < 4){
		opcion = opcion;
	}
	else{
		while (opcion > 4){		
			cout<<"Por favor ingrese un numero de entre el 1 al 4 "<<endl;
			cin>>opcion;
		}
	}
	
}

int main() {
	
	bool lecturaIniciada = false;
    int opcion;
	cout<<"---MENU DE OPCIONES---"<<endl;
	cout<<"1.Leer Datos"<<endl<<"2.Mostrar"<< endl <<"3.Despachar"<< endl <<"4.Salir"<<endl;
	cout<<"Elija una opcion:"<<endl;
	pedirNumero(opcion);
	
	
	
	Nodo<Registro> *listaid = nullptr;
	Nodo<Registro> *listaord = nullptr;
	Registro reg;
    fstream archi;
	

	

	
	while (opcion != 4){

		
			switch (opcion)
	{
	case 1 :
		lecturaIniciada = true;
	cout<<"Mostrando los datos de la lista por defecto : \n"<<endl;
			archi.open("Datos.bin", ios::in | ios::binary);
		if (!archi) {
			cout << "No se pudo abrir el archivo de lectura "
		     "Datos.bin" << endl;
			return EXIT_FAILURE;
		}
		while (archi >> reg) { // rellenamos listas
		insertar(reg, listaid, critid);
		insertar(reg, listaord, critclieprod);
		cout << reg << endl;
		}
		
	archi.close();
		//mostrar(listaid);
		break;
	case 2:
		if(lecturaIniciada){
	cout<<"Mostrando los datos de la lista de pedidos: \n"<<endl;	
			mostrar(listaord);
	cout<<"Lista de Despachos: \n"<<endl;}
	else{cout<<"Aun no se ha leido el archivo"<<endl;
	}
	
		//mostrar(listadespachos)
		break;
	case 3:
		break;
	case 4:
		break;
	
	default:
	
		
		//NO SE SI ESTA BIEN ESTO LA VERDAD, PORQUE HABRIA QUE HACER QUE VUELVA A PEDIR UN NUMERO Y CREO QUE NO ESTARIA PASANDO
		//ahi tire arriba tire una idea para hacerlo con un while, pero habria que acomodarlo creo.
		break;
	}
			pedirNumero(opcion);
	}
}
