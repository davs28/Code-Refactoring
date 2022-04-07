# Extracting Methods, Fields, Classes

```Java
public class Customer {

  private String firstName = "";
  private String lastName = "";
  private String street = "";
  private String city = "";
  private String state = "";
  private int postalCode = 0;
  private String birthDay = "";
  
  // getters and setters
  public String getFirstName() { return firstName; }

  public String getLastName() { return lastName; }

  public String getStreet() { return street; }

  public String getCity() { return city; }

  public String getState() { return state; }

  public int getPostalCode() { return postalCode; }

  public String getBirthDay() { return birthDay; }

  public void setFirstName(String firstName) { this.firstName = firstName; }

  public void setLastName(String lastName) { this.lastName = lastName; }

  public void setStreet(String street) { this.street = street; }

  public void setCity(String city) { this.city = city; }

  public void setState(String state) { this.state = state; }
  
  public void setPostalCode(int postalCode) { this.postalCode = postalCode; }

  public void setBirthDay(String birthDay) { this.birthDay = birthDay; }
  
  public Customer(String firstName, String lastName, String street, String city, String state,
      int postalCode, String birthDay) {
    super();
    this.firstName = firstName;
    this.lastName = lastName;
    this.street = street;
    this.city = city;
    this.state = state;
    this.postalCode = postalCode;
    this.birthDay = birthDay;
  }
}
```
From the above code, we can see that we can extract the street, city, state and postalCode variables
and make them into their own class. This would help with making the customer class smaller and 
easier to digest. Also, it helps reduce the amount of parameters in the Customer constructor.
```Java
class Address {
  
  private String street = "";
  private String city = "";
  private String state = "";
  private int postalCode = 0;
  
  // getters and setters
  public String getStreet() { return street; }

  public String getCity() { return city; }

  public String getState() { return state; }

  public int getPostalCode() { return postalCode; }

  public void setStreet(String street) { this.street = street; }

  public void setCity(String city) { this.city = city; }

  public void setState(String state) { this.state = state; }

  public void setPostalCode(int postalCode) { this.postalCode = postalCode; }
  
  public Address(String street, String city, String state, int postalCode) {
    super();
    this.street = street;
    this.city = city;
    this.state = state;
    this.postalCode = postalCode;
  }
}
```
### After extracting the Address class
We can see now that the Customer class is easier to read and much better overall compared to the 
previous code.
```Java
public class Customer {

  private String firstName = "";
  private String lastName = "";
  private Address address = null;
  private String birthDay = "";
  
  // getters and setters
  public String getFirstName() { return firstName; }

  public String getLastName() { return lastName; }

  public String getBirthDay() { return birthDay; }

  public void setFirstName(String firstName) { this.firstName = firstName; }

  public void setLastName(String lastName) { this.lastName = lastName; }

  public void setBirthDay(String birthDay) { this.birthDay = birthDay; }
  
  public Customer(String firstName, String lastName, Address address, String birthDay) {
    super();
    this.firstName = firstName;
    this.lastName = lastName;
    this.address = address;
    this.birthDay = birthDay;
  }
}
```
Main method in the Customer class:
```Java
public class Customer {
  // ...
  
  public static void main(String[] args) {

    Address sallySmithAddress = new Address("123 Main St", "Perry", "Iowa", 50220);

    Customer sallySmith = new Customer("Sally", "Smith", sallySmithAddress, "12/12/74");

    System.out.println("Customer name: " + sallySmith.getFirstName() + " " + sallySmith.getLastName());

    System.out.println("Customer address: " + sallySmith.address);
  }
}
```
We can see that the code to print out the customer's name is long and might not be very intuitive at
first glance. However, the customer's address code is much simpler and easier to understand despite
storing more information than the customer's name.
<br>

This is done by using a toString() method in the Address class: 
```Java
public class Address {
  // ...
  
  public String toString() {
    return getStreet() + " " + getCity() + " " + getState() + " " + getPostalCode();
  }
}
```
Next, instead of having the birthDay variable as a String, it makes more sense to have it as its own 
separate object. We can do that by making a Birthday class:
```Java
class Birthday {

  private int day;
  private int month;
  private int year;

  public int getDay() { return day; }

  public int getMonth() { return month; }

  public int getYear() { return year; }

  public void setDay(int day) { this.day = day; }

  public void setMonth(int month) { this.month = month; }

  public void setYear(int year) { this.year = year; }
  
  public Birthday(int day, int month, int year) {
    super();
    this.day = day;
    this.month = month;
    this.year = year;
  }
  
  public String getBirthDate() {
    return getDay() + "/" + getMonth() + "/" + getYear();
  }
  
  public String toString() {
    return getDay() + "/" + getMonth() + "/" + getYear();
  }
}
```
The finished refactored Customer class:
```Java
public class Customer {

  private String firstName = "";
  private String lastName = "";
  private Address address = null;
  private Birthday birthDay = null;
  
  // getters and setters
  public String getFirstName() { return firstName; }

  public String getLastName() { return lastName; }

  public void setFirstName(String firstName) { this.firstName = firstName; }

  public void setLastName(String lastName) { this.lastName = lastName; }
  
  public Customer(String firstName, String lastName, Address address, Birthday birthDay) {
    super();
    this.firstName = firstName;
    this.lastName = lastName;
    this.address = address;
    this.birthDay = birthDay;
  }

  public static void main(String[] args) {

    Address sallySmithAddress = new Address("123 Main St", "Perry", "Iowa", 50220);
    
    Birthday sallySmithBirthday = new Birthday(12, 21, 1974);

    Customer sallySmith = new Customer("Sally", "Smith", sallySmithAddress, sallySmithBirthday);

    System.out.println("Customer name: " + sallySmith.getFirstName() + " " + sallySmith.getLastName());

    System.out.println("Customer address: " + sallySmith.address);

    System.out.println("Customer birthday: " + sallySmith.birthDay);
  }
}
```