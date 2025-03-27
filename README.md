# 4. LINQ (Módulo CSharp)

Vamos a partir de estos datos:

```
[
  {
    "id": 1,
    "name": "John",
    "lastname": "Doe",
    "sex": "Male",
    "temperature": 36.8,
    "heartRate": 80,
    "specialty": "general medicine",
    "age": 44,
  },
  {
    "id": 2,
    "name": "Jane",
    "lastname": "Doe",
    "sex": "Female",
    "temperature": 36.8,
    "heartRate": 70,
    "specialty": "general medicine",
    "age": 43,
  },
  {
    "id": 3,
    "name": "Junior",
    "lastname": "Doe",
    "sex": "Male",
    "temperature": 36.8,
    "heartRate": 90,
    "specialty": "pediatrics",
    "age": 8,
  },
  {
    "id": 4,
    "name": "Mary",
    "lastname": "Wien",
    "sex": "Female",
    "temperature": 36.8,
    "heartRate": 120,
    "specialty": "general medicine",
    "age": 20,
  },
  {
    "id": 5,
    "name": "Scarlett",
    "lastname": "Somez",
    "sex": "Female",
    "temperature": 36.8,
    "heartRate": 110,
    "specialty": "general medicine",
    "age": 30,
  },
  {
    "id": 6,
    "name": "Brian",
    "lastname": "Kid",
    "sex": "Male",
    "temperature": 39.8,
    "heartRate": 80,
    "specialty": "pediatrics",
    "age": 11,
  },
]
```

Como primer paso, habrá que crear la clase Patient y la colección para almacenar dichos datos.

1. Extraer la lista de pacientes que sean de la especialidad pediatrics y que tengan menos de 10 años.

2. Queremos activar el protocolo de urgencia si hay algún paciente con ritmo cardíaco mayor que 100 o temperatura mayor a 39.

3. No podemos atender a todos los pacientes hoy por lo que vamos a crear una nueva coleccion y reasignar a los pacientes de pediatrics a general medicine

4. Queremos conocer de una sola consulta el numero de pacientes que estan asignado a general medicine y a pediatrics.

5. Devuelve una lista nueva que por cada paciente se indique si tiene prioridad o no. Se tendrá prioridad si el ritmo cardiaco es mayor 100 o la temperatura es mayor a 39.

6. Queremos conocer de una sola consulta La edad media de pacientes asignados a pediatrics y general medicine.

```csharp
using System.Linq;

namespace curso_csharp
{
    internal class Program
    {
        public class Patient
        {
            public int id {  get; set; }
            public String name { get; set; }
            public String lastname { get; set; }
            public String sex { get; set; }
            public Double temperature { get; set; }
            public int heartRate { get; set; }
            public String specialty { get; set; }
            public int age { get; set; }

            public Patient(int id, string name, string lastname, string sex, double temperature, int heartRate, string specialty, int age)
            {
                this.id = id;
                this.name = name;
                this.lastname = lastname;
                this.sex = sex;
                this.temperature = temperature;
                this.heartRate = heartRate;
                this.specialty = specialty;
                this.age = age;
            }


        }
        static void Main(string[] args)
        {
            List<Patient> list = new List<Patient>();

            Patient p1 = new Patient(1, "John", "Doe", "Male", 36.8, 80, "general medicine", 44);
            Patient p2 = new Patient(2, "Jane", "Doe", "Female", 36.8, 70, "general medicine", 43);
            Patient p3 = new Patient(3, "Junior", "Doe", "Male", 36.8, 90, "pediatrics", 8);
            Patient p4 = new Patient(4, "Mary", "Wien", "Female", 36.8, 120, "general medicine", 20);
            Patient p5 = new Patient(5, "Scarlett", "Somez", "Female", 36.8, 110, "general medicine", 30);
            Patient p6 = new Patient(6, "Brian", "Kid", "Male", 39.8, 80, "pediatrics", 11);

            list.Add(p1);
            list.Add(p2);
            list.Add(p3);
            list.Add(p4);
            list.Add(p5);
            list.Add(p6);

            // 1 - Extraer la lista de pacientes que sean de la especialidad pediatrics y que tengan menos de 10 años.
            Console.WriteLine("1 - Extraer la lista de pacientes que sean de la especialidad pediatrics y que tengan menos de 10 años.");
            var pedriaticsList = list.Where(p => p.specialty.Equals("pediatrics") && p.age <= 10).Select(p => $"{p.name}, {p.lastname}");

            foreach(var pedriatic in pedriaticsList)
            {
                Console.WriteLine(pedriatic);
            }
            Console.WriteLine();

            // 2 - Queremos activar el protocolo de urgencia si hay algún paciente con ritmo cardíaco mayor que 100 o temperatura mayor a 39.
            Console.WriteLine("2 - Queremos activar el protocolo de urgencia si hay algún paciente con ritmo cardíaco mayor que 100 o temperatura mayor a 39.");
            var urgency = list.Any(p => p.heartRate > 100 || p.temperature > 39);

            if (urgency == true)
            {
                Console.WriteLine("Protocolo de urgencia activado!!!");
            } else
            {
                Console.WriteLine("Protocolo de urgencia inactivo.");
            }
            Console.WriteLine();

            // 3 - No podemos atender a todos los pacientes hoy por lo que vamos a crear una nueva coleccion y reasignar a los pacientes de pediatrics a general medicine
            Console.WriteLine("3 - No podemos atender a todos los pacientes hoy por lo que vamos a crear una nueva coleccion y reasignar a los pacientes de pediatrics a general medicine");
            var pedriaticsList2 = list.Where(p => p.specialty.Equals("pediatrics"));

            List<Patient> generalMedicineList = new List<Patient>();
            generalMedicineList.Add(p1);
            generalMedicineList.Add(p2);
            generalMedicineList.Add(p4);
            generalMedicineList.Add(p5);

            foreach(var pedriatic in pedriaticsList2)
            {
                Patient p = new Patient(pedriatic.id, pedriatic.name,
                    pedriatic.lastname, pedriatic.sex, pedriatic.temperature,
                    pedriatic.heartRate, "general medicine", pedriatic.age);

                generalMedicineList.Add(p);
            }

            var nameAndSpecialty = generalMedicineList.Select(p => $"{p.name}, {p.lastname} --> {p.specialty}");

            foreach(var p in nameAndSpecialty)
            {
                Console.WriteLine(p);
            }
            Console.WriteLine();

            // 4 - Queremos conocer de una sola consulta el numero de pacientes que estan asignado a general medicine y a pediatrics.
            Console.WriteLine("4 - Queremos conocer de una sola consulta el numero de pacientes que estan asignado a general medicine y a pediatrics.");
            var numGMandPedratics = list.GroupBy(p => p.specialty).Select(p => new {Specialty = p.Key, Count = p.Count()});

            foreach (var p in numGMandPedratics)
            {
                Console.WriteLine($"{p.Specialty} --> {p.Count}");
            }
            Console.WriteLine();

            // 5 - Devuelve una lista nueva que por cada paciente se indique si tiene prioridad o no. Se tendrá prioridad si el ritmo cardiaco es mayor 100 o la temperatura es mayor a 39.
            Console.WriteLine("5 - Devuelve una lista nueva que por cada paciente se indique si tiene prioridad o no. Se tendrá prioridad si el ritmo cardiaco es mayor 100 o la temperatura es mayor a 39.");

            var priorityList = list.Select(p => new { p.name, p.lastname, Priority = p.heartRate > 100 || p.temperature > 39 });

            foreach (var patient in priorityList)
            {
                Console.WriteLine($"{patient.name}, {patient.lastname} --> Prioridad: {patient.Priority}");
            }
            Console.WriteLine();

            // 6 - Queremos conocer de una sola consulta La edad media de pacientes asignados a pediatrics y general medicine.
            Console.WriteLine("6 - Queremos conocer de una sola consulta La edad media de pacientes asignados a pediatrics y general medicine.");
            var mediaGMandPedratics = list.GroupBy(p => p.specialty).Select(p => new {Specialty = p.Key, Average = p.Average(p => p.age)});

            foreach (var p in mediaGMandPedratics)
            {
                Console.WriteLine($"{p.Specialty} --> {(int) p.Average}");
            }
            Console.ReadLine();
        }
    }
}
```