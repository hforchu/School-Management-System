# School-management-system

#
# Student class
# creates a student and the functionalities around the student

import java.util.Random;

public class Student {

	private String name;
	private int year;
	private String studentIDNumber = ""; // using a string because I am concatenating random ints into the ID
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
		if (numberOfCourses > School_management_system.maxCourses)
			this.numberOfCourses = School_management_system.maxCourses;
		else
			this.numberOfCourses = numberOfCourses;
		generateStudentIDNumber();
	}

	/**
	 * constructor used in next method in StudentList to make everyone a student
	 * 
	 * @param name
	 * @param year
	 * @param numCourses
	 * @param IDNumber
	 */
	public Student(String name, int year, int numCourses, String IDNumber) {
		this.name = name;
		this.year = year;
		this.numberOfCourses = numCourses;
		studentIDNumber = IDNumber;
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
		}
		list.addStudentIDNumber(studentIDNumber);
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
	 * @param amountPaying, the money to be deducted from the current balance
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

	/**
	 * returns the student's ID number
	 * 
	 * @return
	 */
	public String getIDNumber() {
		return studentIDNumber;
	}

	/**
	 * gives the student's information in a presentable, easy to read format
	 */
	public void studentInfo() {

		if (getYear() == 1) {
			System.out.println(this.getName() + " is in their first year, and is taking " + getNumCourses()
					+ " courses. Student ID is " + this.getIDNumber());
		} else if (getYear() == 2) {
			System.out.println(this.getName() + " is in their second year, and is taking " + getNumCourses()
					+ " courses. Student ID is " + this.getIDNumber());
		} else if (getYear() == 3) {
			System.out.println(this.getName() + " is in their third year, and is taking " + getNumCourses()
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
	private int index, i;
	private ArrayList<String> stringItems = new ArrayList<>(); // used to store the names and Student ID numbers of everyone who is added to the list
	private ArrayList<Integer> intItems = new ArrayList<>(); // used to store the year and number of courses to everyone who is added to the list

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

		Student newStudent = new Student(name, year, numCourses);
		addStudent(newStudent);
	}

	/**
	 * adds an already made student to the list of students in the school system
	 * 
	 * @param name
	 */
	public void addStudent(Student name) {

		if (!list.contains(name.getName())) {
			list.add(name.getName());
			stringItems.add(name.getName());
			intItems.add(name.getYear());
			intItems.add(name.getNumCourses());
			stringItems.add(name.getIDNumber());
		}
	}

	/**
	 * removes a person from the list of students in that school
	 * 
	 * @param name
	 * @return
	 */
	public void remove(String name) {

		list.remove(name);
		stringItems.remove(stringItems.indexOf(name) + 1);
		intItems.remove(stringItems.indexOf(name) + 1);
		intItems.remove(stringItems.indexOf(name));
		stringItems.remove(name);
		index = i = 0;
	}

	/**
	 * removes a student from the list of students
	 * 
	 * @param name
	 */
	public void removeStudent(Student name) {

		list.remove(name.getName());
		stringItems.remove(stringItems.indexOf(name.getName()) + 1);
		intItems.remove(stringItems.indexOf(name.getName()) + 1);
		intItems.remove(stringItems.indexOf(name.getName()));
		stringItems.remove(name.getName());
		index = i = 0;
	}

	/**
	 * adds the used ID Numbers to a list so no two people have the same ID number
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
			next = new Student(stringItems.get(i), intItems.get(i), intItems.get(i + 1), stringItems.get(i + 1));
			index++;
			i += 2;
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
		Student luke = new Student("Luke Jones", 2, 4);
		list.addStudent(joe);
		list.add("Chris Paul", 1, 6);
		list.addStudent(luke);
		list.add("Suzie Sue", 3, 3);
		list.addStudent(tomas);
		list.add("Michael Mayer", 4, 2);

		list.listStudents();

		System.out.println();
		for (int i = 0; i < list.getSize(); i++) {
			list.next().studentInfo();
		}

		System.out.println("\nJoe's tuition is $" + joe.calulateTuition(joe.getNumCourses()));
		joe.payTuition(2000);
		System.out.println("Now, Joe's tuition is $" + joe.getBalance() + " after paying part of his tuition.");

		System.out.println("\n" + list.getSize());
		list.remove("Chris Paul");
		list.removeStudent(joe);
		System.out.println(list.getSize());
		list.listStudents();

		System.out.println("\n");
		for (int i = 0; i < list.getSize(); i++) {
			list.next().studentInfo();
		}

	}
}
