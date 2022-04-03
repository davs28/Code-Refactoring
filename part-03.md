# Using Variables to Write Understandable Code

Product class:
 ```Java
 public class Product {
  
  private String name = "";
  private double price = 0.0;
  private double shippingCost = 0.0;
  private int quantity = 0;
  
  public String getName() { return name; }
  
  public double getPrice() { return price; }
  
  public double getShippingCost() { return shippingCost; }
  
  public int getQuantity() { return quantity; }
  
  Product(String name, double price, double shippingCost, int quantity) {
    
    this.name = name;
    this.price = price;
    this.shippingCost = shippingCost;
    this.quantity = quantity;
  }
  
  public double getTotalCost() {
    
    double quantityDiscount = 0.0;
    
    if ((quanity > 50) || ((quantity * price) > 500)) {
      quantityDiscount = .10;
    } else if ((quantity > 25) || ((quantity * price) > 100)) {
      quantityDiscount = .07;
    } else if ((quantity > 10) || ((quantity * price) > 50)) {
      quantityDiscount = .05;
    }
    
    double discount = ((quantity - 1) * quantityDiscount) * price;
  }
}
 ```
In the getTotalCost() method above, instead of using conditionals in the if..else statements,
we can make use of temp variables to define these conditionals to so the code can look much cleaner.
```Java
public double getTotalCost() {
    
  double quantityDiscount = 0.0;
  
  final boolean over50Products = (quantity > 50) || ((quantity * price) > 500);
  final boolean over25Products = (quantity > 25) || ((quantity * price) > 100);
  final boolean over10Products = (quantity > 10) || ((quantity * price) > 50);
    
  if (over50Products) {
    quantityDiscount = .10;
  } else if (over25Products) {
    quantityDiscount = .07;
  } else if (over10Products) {
    quantityDiscount = .05;
  }
  
  double discount = ((quantity - 1) * quantityDiscount) * price;
}
```
<br>

Store class with main method:
```Java
import java.util.ArrayList;

public class Store {
  
  public ArrayList<Product> theProducts = new ArrayList<Product>();
  
  public void addAProduct(Product newProduct) {
    theProducts.add(newProduct);
  }
  
  public void getCostOfProducts() {
    
    for(Product product : theProducts) {
      System.out.println("Total cost for " + product.getQuantity() + " " + product.getName() + "s is $" + product.getTotalCost());
      System.out.println("Cost per product " + product.getTotalCost() / product.getQuantity());
      System.out.println("Savings per product " + ((product.getPrice() + product.getShippingCost()) - (product.getTotalCost() / product.getQuantity())) + "\n");
    }
  }

  public static void main(String[] args) {
    
    Store cornerStore = new Store();
    
    cornerStore.addAProduct(new Product("Pizza", 10.00, 1.00, 52));
    cornerStore.addAProduct(new Product("Pizza", 10.00, 1.00, 26));
    cornerStore.addAProduct(new Product("Pizza", 10.00, 1.00, 10));
    cornerStore.getCostOfProducts();
  }
}
```
Here we can use temp variables to explain what is happening behind the scenes. In getCostOfProducts() 
above, the code for writing the output message was getting too long and convoluted. We can replace 
the methods in those print statements with variables that make more sense to us.
```Java
public void getCostOfProducts() {
    
  for(Product product : theProducts) {
    
    final int numOfProducts = product.getQuantity();
    final String prodName = product.getName();
    final double cost = product.getTotalCost();
    
    final double costWithDiscount = product.getTotalCost() / product.getQuantity();
    final double costWithoutDiscount = product.getPrice() + product.getShippingCost();
    
    System.out.println("Total cost for " + numOfProducts + " " + prodName + "s is $" + cost);
    System.out.println("Cost per product " + costWithDiscount);
    System.out.println("Savings per product " + (costWithoutDiscount - costWithDiscount) + "\n");
  }
}
```
<br>

### Why it's bad to assign multiple values to a temporary variable.
```Java
double temp = totalCost / numberOfProducts; // temp = Individual Product Cost

temp = temp + shipping; // Individual Product Cost + Shipping

temp = temp - discount; // Individual Product Cost + Shipping - Discount
```
Sure, the code above might be pretty clear to you now, but when you revisit this code again in the
future, you might spend some time trying to remember what this code is doing.
<br>

It is better to declare new temp variables with meaningful names to make the code more understandable.
```Java
double indivProductCost = totalCost / numberOfProducts;

double prodCostAndShipping = indivProductCost + shipping;

double discountedProductCost = prodCostAndShipping - discount;
```
Another example:
```Java
public double getTotPrice(double quantity, double price, double shippingCost, double discount) {
  
    price = price + shippingCost;
    price = price * quantity;
    return price - discount
}
```
After refactoring:
```Java
public double getTotPrice(double quantity, double price, double shippingCost, double discount){

    priceAfterShipping = price + shippingCost;
    totalPrice = priceAfterShipping * quantity;
    discountedTotalPrice = totalPrice - discount;
    return discountedPrice;
}
```