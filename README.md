import java.io.*;
import java.util.*;

class Student implements Serializable {
    int studentID;
    String name;
    String grade;

    Student(int studentID, String name, String grade) {
        this.studentID = studentID;
        this.name = name;
        this.grade = grade;
    }
}

class Employee {
    int id;
    String name;
    String designation;
    double salary;

    Employee(int id, String name, String designation, double salary) {
        this.id = id;
        this.name = name;
        this.designation = designation;
        this.salary = salary;
    }

    public String toString() {
        return id + "," + name + "," + designation + "," + salary;
    }
}

public class MainProgram {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (true) {
            System.out.println("\n--- MAIN MENU ---");
            System.out.println("1. Sum of Integers (Autoboxing & Unboxing)");
            System.out.println("2. Student Serialization & Deserialization");
            System.out.println("3. Employee Management (File Handling)");
            System.out.println("4. Exit");
            System.out.print("Enter choice: ");
            int choice = sc.nextInt();
            sc.nextLine();

            switch (choice) {
                case 1:
                    sumOfIntegers();
                    break;
                case 2:
                    studentSerialization();
                    break;
                case 3:
                    employeeManagement();
                    break;
                case 4:
                    System.out.println("Exiting program...");
                    return;
                default:
                    System.out.println("Invalid choice.");
            }
        }
    }

    static void sumOfIntegers() {
        Scanner sc = new Scanner(System.in);
        ArrayList<Integer> numbers = new ArrayList<>();
        System.out.println("Enter numbers (type 'end' to stop):");
        while (true) {
            String input = sc.next();
            if (input.equalsIgnoreCase("end")) break;
            Integer num = Integer.parseInt(input);
            numbers.add(num);
        }
        int sum = 0;
        for (int n : numbers) sum += n;
        System.out.println("Sum: " + sum);
    }

    static void studentSerialization() {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter Student ID: ");
        int id = sc.nextInt();
        sc.nextLine();
        System.out.print("Enter Name: ");
        String name = sc.nextLine();
        System.out.print("Enter Grade: ");
        String grade = sc.nextLine();

        Student s = new Student(id, name, grade);
        try {
            ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("student.ser"));
            oos.writeObject(s);
            oos.close();
            ObjectInputStream ois = new ObjectInputStream(new FileInputStream("student.ser"));
            Student s2 = (Student) ois.readObject();
            ois.close();
            System.out.println("\nDeserialized Student:");
            System.out.println("ID: " + s2.studentID);
            System.out.println("Name: " + s2.name);
            System.out.println("Grade: " + s2.grade);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void employeeManagement() {
        Scanner sc = new Scanner(System.in);
        while (true) {
            System.out.println("\n--- EMPLOYEE MENU ---");
            System.out.println("1. Add Employee");
            System.out.println("2. Display All Employees");
            System.out.println("3. Back to Main Menu");
            System.out.print("Enter choice: ");
            int ch = sc.nextInt();
            sc.nextLine();
            if (ch == 1) {
                System.out.print("Enter ID: ");
                int id = sc.nextInt();
                sc.nextLine();
                System.out.print("Enter Name: ");
                String name = sc.nextLine();
                System.out.print("Enter Designation: ");
                String des = sc.nextLine();
                System.out.print("Enter Salary: ");
                double sal = sc.nextDouble();
                try {
                    BufferedWriter bw = new BufferedWriter(new FileWriter("employees.txt", true));
                    bw.write(id + "," + name + "," + des + "," + sal);
                    bw.newLine();
                    bw.close();
                    System.out.println("Employee added successfully.");
                } catch (IOException e) {
                    e.printStackTrace();
                }
            } else if (ch == 2) {
                try {
                    BufferedReader br = new BufferedReader(new FileReader("employees.txt"));
                    String line;
                    System.out.println("\nEmployee Records:");
                    while ((line = br.readLine()) != null) {
                        String[] data = line.split(",");
                        System.out.println("ID: " + data[0] + ", Name: " + data[1] + ", Designation: " + data[2] + ", Salary: " + data[3]);
                    }
                    br.close();
                } catch (IOException e) {
                    System.out.println("No records found.");
                }
            } else if (ch == 3) {
                break;
            } else {
                System.out.println("Invalid choice.");
            }
        }
    }
}
