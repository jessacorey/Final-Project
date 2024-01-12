// Student.java
package finalproject;

// Class representing a Student with name, address, and GPA
public class Student implements Comparable<Student> {
    private String name;
    private String address;
    private double GPA;

    // Constructor to initialize a Student object
    public Student(String name, String address, double GPA) {
        this.name = name;
        this.address = address;
        this.GPA = GPA;
    }

    // Getter methods for accessing private fields
    public String getName() {
        return name;
    }

    public String getAddress() {
        return address;
    }

    public double getGPA() {
        return GPA;
    }

    // Method to define natural ordering based on name for sorting
    @Override
    public int compareTo(Student other) {
        return this.name.compareTo(other.getName());
    }
}
// DataProcessor.java
package finalproject;

import java.util.List;
import java.util.Scanner;

// Interface defining methods for processing student data and writing to a file
interface DataProcessor {
    Student process(Scanner scanner);
    void writeToFile(List<Student> studentList);
}
// StudentDataEntry.java
package finalproject;

import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.util.Collections;
import java.util.LinkedList;
import java.util.List;
import java.util.Scanner;

// Class implementing DataProcessor interface to handle student data entry and processing
public class StudentDataEntry implements DataProcessor {
    public static void main(String[] args) {
        List<Student> studentList = new LinkedList<>();
        Scanner scanner = new Scanner(System.in);
        DataProcessor dataProcessor = new StudentDataEntry();

        // Prompt user for student data entry
        while (true) {
            System.out.println("Enter student data (or type 'exit' to finish):");
            Student student = dataProcessor.process(scanner);

            if (student == null) {
                break;
            }

            studentList.add(student);
        }

        // Sort the list by name
        Collections.sort(studentList);

        // Write to a text file
        dataProcessor.writeToFile(studentList);

        scanner.close();
    }

    // Method to process individual student data
    @Override
    public Student process(Scanner scanner) {
        System.out.print("Name: ");
        String name = scanner.nextLine();

        if (name.equalsIgnoreCase("exit")) {
            return null;
        }

        System.out.print("Address: ");
        String address = scanner.nextLine();

        double GPA;
        while (true) {
            System.out.print("GPA: ");
            try {
                GPA = Double.parseDouble(scanner.nextLine());
                // Validate GPA (Assuming GPA is between 0 and 4)
                if (GPA < 0 || GPA > 4) {
                    System.out.println("Please enter a valid GPA between 0 and 4.");
                    continue;
                }
                break;
            } catch (NumberFormatException e) {
                System.out.println("Invalid GPA. Please enter a numeric value.");
            }
        }

        return new Student(name, address, GPA);
    }

    // Method to write student data to a text file
    @Override
    public void writeToFile(List<Student> studentList) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("student_data.txt"))) {
            for (Student student : studentList) {
                writer.write(String.format("Name: %s\tAddress: %s\tGPA: %.2f", student.getName(), student.getAddress(), student.getGPA()));
                writer.newLine();
            }
            System.out.println("Student data has been written to 'student_data.txt'");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
