# School-management-system
# Work in progress



# Student class
# creates a student and the functionalities around the student

import java.util.Random;

public class Student {

	private String name;
	private int year;
	private String studentIDNumber = "";
	private int numberOfCourses;
	private int tuitionBalance;
	private StudentList list = new StudentList();

	/**
	 * the constructor for student, takes the following parameters
	 * 
	 * @param name,            the name of the student
	 * @param year,            the year the student is in
	 * @param numberOfCOurses, the number of courses the student is enrolled in
	 */
	public Student(String name, int year, int numberOfCourses) {
		this.name = name;
		this.year = year;
		this.numberOfCourses = numberOfCourses;
		list.add(name, year, numberOfCourses);
		generateStudentIDNumber();
	}

	/**
	 * 
	 * @param name
	 */
	public Student(String name) {
		this.name = name;
		this.year = getYear();
		this.numberOfCourses = getNumCourses();
		list.add(name, year, numberOfCourses);
		studentIDNumber = this.getIDNumber();
	}

	/**
	 * creates a random student ID number of six digits for a new student
	 * 
	 * @return
	 */
	private String generateStudentIDNumber() {

		Random num = new Random();
		for (int i = 0; i < 6; i++) {
			studentIDNumber += num.nextInt(10);
		}
		if (list.usedIDNumbers.contains(studentIDNumber)) {
			generateStudentIDNumber();
		} else {
			list.addStudentIDNumber(studentIDNumber);
		}
		return studentIDNumber;
	}

	/**
	 * calculates the total tuition each student pays
	 * 
	 * @param numberofCourses, the number of courses each student is taking
	 * @return
	 */
	public int calulateTuition(int numberofCourses) {

		tuitionBalance = numberofCourses * School_management_system.feePerCourse;
		return tuitionBalance;
	}

	/**
	 * adjusts the balance of the student's tuition
	 * 
	 * @param amountPaying,
	 */
	public void payTuition(int amountPaying) {

		if (amountPaying > tuitionBalance)
			tuitionBalance = 0;
		else
			tuitionBalance -= amountPaying;
	}

	/**
	 * returns the name of the student
	 * 
	 * @return
	 */
	public String getName() {
		return name;
	}

	/**
	 * Returns the remaining tuition the student has to pay.
	 * 
	 * @return
	 */
	public int getBalance() {

		return tuitionBalance;
	}

	/**
	 * returns the number of courses the student is taking number of courses is
	 * randomly generated
	 * 
	 * @return
	 */
	public int getNumCourses() {
		return numberOfCourses;
	}

	/**
	 * returns the year the student is in year is randomly generated
	 * 
	 * @return
	 */
	public int getYear() {
		return year;
	}

	public String getIDNumber() {
		return studentIDNumber;
	}

	/**
	 * gives the student's information in a presentable, easy to read format
	 */
	public void studentInfo() {

		if (getYear() == 1) {
			System.out.println(this.getName() + ": Student is in their first year, and is taking " + getNumCourses()
					+ " courses. Student ID is " + this.getIDNumber());
		} else if (getYear() == 2) {
			System.out.println(this.getName() + ": Student is in their second year, and is taking " + getNumCourses()
					+ " courses. Student ID is " + this.getIDNumber());
		} else if (getYear() == 3) {
			System.out.println(this.getName() + ": Student is in their third year, and is taking " + getNumCourses()
					+ " courses. Student ID is " + this.getIDNumber());
		} else {
			System.out.println(this.getName() + " is in their final year, and is taking " + getNumCourses()
					+ " courses. Student ID is " + this.getIDNumber());
		}
	}
}




# StudentList class
# creates a list of students in the system

import java.util.ArrayList;
import java.util.NoSuchElementException;

public class StudentList {

	private ArrayList<String> list;
	public ArrayList<String> usedIDNumbers;
	int index;

	/**
	 * constructor for building the list of students in the school
	 * 
	 * @param schoolName
	 */
	public StudentList() {

		list = new ArrayList<>();
		usedIDNumbers = new ArrayList<>();
	}

	/**
	 * adds a new person to the list of students in the school system
	 * 
	 * @param name
	 * @param year
	 * @param numCourses
	 */
	public void add(String name, int year, int numCourses) {

		list.add(name);
	}

	/**
	 * adds an already made student to the list of students in the school system
	 * 
	 * @param name
	 */
	public void addStudent(Student name) {

		add(name.getName(), name.getYear(), name.getNumCourses());
	}

	/**
	 * removes a person from the list of students in that school
	 * 
	 * @param name
	 * @return
	 */
	public void remove(String name) {

		list.remove(name);
	}

	/**
	 * removes a student from the list of students
	 * 
	 * @param name
	 */
	public void removeStudent(Student name) {
		list.remove(name.getName());
	}

	/**
	 * adds the used UD Numbers to a list so no two people have the same ID number
	 * 
	 * @param studentIDNumber
	 */
	public void addStudentIDNumber(String studentIDNumber) {

		usedIDNumbers.add(studentIDNumber);
	}

	public int getSize() {

		return list.size();
	}

	/**
	 * checks if there is another student in the list
	 * 
	 * @return
	 */
	private boolean hasNext() {

		return index < list.size();
	}

	/**
	 * returns the next student in the list, and moves the index to the next
	 * position
	 * 
	 * @return
	 */
	public Student next() {

		Student next;
		if (hasNext()) {
			next = new Student(list.get(index));
			index++;
			return next;
		}
		throw new NoSuchElementException();
	}

	/**
	 * returns a list of all the students in the system
	 */
	public void listStudents() {
		for (int i = 0; i < list.size(); i++) {
			System.out.println(list.get(i));
		}
	}

}


# Test Class

public class School_management_system {

	static StudentList list = new StudentList();
	public static final int feePerCourse = 600;
	public static final int maxCourses = 5;

	/**
	 * main method testing the different parts of the school management system
	 * 
	 * @param args
	 */
	public static void main(String[] args) {

		Student joe = new Student("Joe Harris", 4, 5);
		Student tomas = new Student("Tomas Jones", 1, 4);
		list.addStudent(joe);
		list.add("Chris Paul", 1, 5);
		list.add("Suzie Sue", 3, 3);
		list.addStudent(tomas);

		list.listStudents();

		System.out.println();
		for (int i = 0; i < list.getSize(); i++) {
			list.next().studentInfo();
		}

		System.out.println();
		tomas.studentInfo();

		System.out.println("\nJoe's tuition is $" + joe.calulateTuition(joe.getNumCourses()));
		joe.payTuition(2000);
		System.out.println("Now, Joe's tuition is $" + joe.getBalance() + " after paying part of his tuition.");

		System.out.println("\n" + list.getSize());
		list.remove("Chris Paul");
		list.removeStudent(joe);
		System.out.println(list.getSize());
	}
}

