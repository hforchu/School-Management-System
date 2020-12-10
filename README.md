# School-management-system, Work in progress



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
	 * 
	 * @param name, the name of the student
	 * @param year, the year the student is in
	 */
	public Student(String name, int year, int numberOfCourses) {
		this.name = name;
		this.year = year;
		this.numberOfCourses = numberOfCourses;
		list.addStudent(name, year, numberOfCourses);
		generateStudentIDNumber();
	}

	public Student(String name) {
		this.name = name;
		this.year = getYear();
		this.numberOfCourses = getNumCourses();
		list.addStudent(name, year, numberOfCourses);
		generateStudentIDNumber();
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

	/**
	 * gives the student's information in a presentable easy to read format
	 */
	public void studentInfo() {

		if (year == 1) {
			System.out.println(this.getName() + ": Student is in their first year, and taking " + getNumCourses()
					+ " courses. Student ID is " + studentIDNumber);
		} else if (year == 2) {
			System.out.println(this.getName() + ": Student is in their second year, and taking " + getNumCourses()
					+ " courses. Student ID is " + studentIDNumber);
		} else if (year == 3) {
			System.out.println(this.getName() + ": Student is in their third year, and taking " + getNumCourses()
					+ " courses. Student ID is " + studentIDNumber);
		} else {
			System.out.println(this.getName() + ": Student is in their final year, and taking " + getNumCourses()
					+ " courses. Student ID is " + studentIDNumber);
		}
	}
}



# StudentList class

import java.util.ArrayList;
import java.util.NoSuchElementException;

public class StudentList {

	private ArrayList<String> list;
	public ArrayList<String> usedIDNumbers;
	int index;

	/**
	 * contructor for building the list of students in the school
	 * 
	 * @param schoolName
	 */
	public StudentList() {
		list = new ArrayList<>();
		usedIDNumbers = new ArrayList<>();
	}

	/**
	 * adds the student to the list of students in the school system
	 * 
	 * @param name
	 * @param year
	 * @param numCourses
	 */
	public void addStudent(String name, int year, int numCourses) {
		list.add(name);
	}

	public void add(Student name) {
		addStudent(name.getName(), name.getYear(), name.getNumCourses());
	}

	/**
	 * removes a student from the list of students in that school
	 * 
	 * @param name
	 * @return
	 */
	public void remove(String name) {

		list.remove(name);
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
	 * 
	 * @return
	 */
	private boolean hasNext() {
		// TODO Auto-generated method stub
		return index < list.size();
	}

	/**
	 * 
	 * @return
	 */
	public Student next() {
		// TODO Auto-generated method stub
		Student next;
		if (hasNext()) {
			next = new Student(list.get(index));
			index++;
			return next;
		}
		throw new NoSuchElementException();
	}

}

# Test Class, work in progress

public class School_management_system {

	static StudentList list = new StudentList();
	public static final int feePerCourse = 600;
	public static final int maxCourses = 5;

	/**
	 * 
	 * @param args
	 */
	public static void main(String[] args) {

		Student joe = new Student("Joe Harris", 4, 5);
		list.add(joe);
		list.addStudent("Chris Paul", 1, 5);
		list.addStudent("Suzie Sue", 3, 3);
		joe.studentInfo();

		for (int i = 0; i < list.getSize(); i++) {
			list.next().studentInfo();
		}
		
		System.out.println(list.getSize());
		list.remove("Chris Paul");
		System.out.println(list.getSize());
	}
}
