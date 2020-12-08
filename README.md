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
			list.usedIDNumbers.add(studentIDNumber);
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

		tuitionBalance = numberofCourses * Database.feePerCourse;
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
# class is iterable

import java.util.ArrayList;
import java.util.Iterator;
import java.util.NoSuchElementException;

public class StudentList implements Iterable<Student> {

	private ArrayList<String> list;
	public ArrayList<String> usedIDNumbers;

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

	@Override
	public Iterator<Student> iterator() {
		// TODO Auto-generated method stub
		return null;
	}
}

class studentListIterator<String> implements Iterator<Student> {

	StudentList list = new StudentList();
	int index;

	public studentListIterator(int position) {
		if (position < 0 || position > list.getSize())
			throw new IndexOutOfBoundsException();
		index = position;
	}

	@Override
	public boolean hasNext() {
		// TODO Auto-generated method stub
		return index < list.getSize();
	}

	@Override
	public Student next() {
		// TODO Auto-generated method stub
		if (hasNext()) {
			index++;
		}
		throw new NoSuchElementException();
	}
}
