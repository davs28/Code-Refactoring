# Replacing Constructors with Factory Methods and More

```Java
public abstract class Customer {

  private String custRating;
  static final int PREMIER = 2;
  static final int VALUED = 1;
  static final int DEADBEAT = 0;

  public String getCustRating() {
    return custRating;
  }

  public void setCustRating(String custRating) {
    this.custRating = custRating;
  }

  public static void main(String[] args) {
    
    CustomerFactory customerFactory = new CustomerFactory();
    
    Customer goodCustomer = customerFactory.getCustomer(PREMIER);

    System.out.println("Customer Rating: " + goodCustomer.getCustRating());
  }
}
```
```Java
class Premier extends Customer {
  Premier() {
    setCustRating("Premier Customer");
  }
}

class Valued extends Customer {
  Valued() {
    setCustRating("Valued Customer");
  }
}

class Deadbeat extends Customer {
  Deadbeat() {
    setCustRating("Deadbeat Customer");
  }
}
```
```Java
class CustomerFactory {
  public Customer getCustomer(int custType) {
    
    switch(custType) {
      
      case 2:
        return new Premier();
      case 1:
        return new Valued();
      case 0:
        return new DeadBeat();
      default:
        throw new IllegalArgumentException("Invalid Customer Type");
    }
  }
}
```
```Java
class CustomerFactory {
  public Customer getCustomer(String custName) {
    
    try {
      return (Customer) Class.forName(custName).newInstance();
    }
    
    catch(Exception e) {
      System.out.println("Invalid Customer Type");
    }
    return null;
  }
}
```
```Java
public abstract class Customer {

  private String custRating;
  static final int PREMIER = 2;
  static final int VALUED = 1;
  static final int DEADBEAT = 0;

  public String getCustRating() {
    return custRating;
  }

  public void setCustRating(String custRating) {
    this.custRating = custRating;
  }

  public static void main(String[] args) {
    
    CustomerFactory customerFactory = new CustomerFactory();
    
    Customer goodCustomer = customerFactory.getCustomer("Deadbeat");

    System.out.println("Customer Rating: " + goodCustomer.getCustRating());
  }
}
```

```Java
public class Athlete {

  private String athleteName = "";

  public String getAthleteName() {
    return athleteName;
  }

  public void setAthleteName(String athleteName) {
    this.athleteName = athleteName;
  }
  
  public static Athlete getInstance() {
    return null;
  }
}
```
```Java
class GoldWinner extends Athlete {
  private static goldWinner goldAthlete = null;
  
  private GoldWinner(String athleteName) {
    setAthleteName(athleteName);
  }
  
  public static GoldWinner getInstance(String athleteName) {
    if (goldAthlete == null) {
      goldAthlete = new GoldWinner(athleteName);
    }
    return goldAthlete;
  }
}

class SilverWinner extends Athlete {
  private static silverWinner silverAthlete = null;

  private SilverWinner(String athleteName) {
    setAthleteName(athleteName);
  }

  public static SilverWinner getInstance(String athleteName) {
    if (silverAthlete == null) {
      silverAthlete = new SilverWinner(athleteName);
    }
    return silverAthlete;
  }
}

class BronzeWinner extends Athlete {
  private static bronzeWinner bronzeAthlete = null;

  private BronzeWinner(String athleteName) {
    setAthleteName(athleteName);
  }

  public static BronzeWinner getInstance(String athleteName) {
    if (bronzeAthlete == null) {
      bronzeAthlete = new BronzeWinner(athleteName);
    }
    return bronzeAthlete;
  }
}
```
```Java
import java.lang.reflect.Method;

class MedalFactory {
  
  public Athlete getMedal(String medalType, String athleteName) {
    try {
      Class[] athleteNameParameter = new Class[] {String.class};
      
      Method getInstanceMethod = Class.forName(medalType).getMethod("getInstance", parameterTypes);
      
      Object[] params = new Object[] {new String(athleteName)};
      
      return (Athlete) getInstanceMethod.invoke(null, params);
    }
    catch(Exception e) {
      throw new IllegalArgumentException("Invalid Athlete Type");
    }
  }
}
```
```Java
class TestMedalWinner {

  public static void main(String[] args) {
    MedalFactory medalFactory = new MedalFactory();
    
    Athlete goldWinner = medalFactory.getMedal("GoldWinner", "Dave Thomas");
    Athlete silverWinner = medalFactory.getMedal("SilverWinner", "Mac Mcdonald");
    Athlete bronzeWinner = medalFactory.getMedal("Bronze Winner", "David Edgerton");
    
    Athlete goldWinnerNew = medalFactory.getMedal("GoldWinner", "Ray Kroc");

    System.out.println("GoldWinner: " + goldWinnerNew.getAthleteName());
    System.out.println("SilverWinner: " + silverWinner.getAthleteName());
    System.out.println("BronzeWinner: " + bronzeWinner.getAthleteName());
  }
}
```