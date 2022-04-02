# Extracting Methods

FootballPlayer class:
```Java
public class FootballPlayer {
    private String name = "";
    private double[] fortyYardDashTimes = null;

    public String getName(){ return name; }

    FootballPlayer(String name, double[] fortyYardDashTimes) {
        this.name = name;
        this.fortyYardDashTimes = fortyYardDashTimes;
    }
}
```
The class which has the main method:
```Java
import java.util.ArrayList;

public class FootballPlayer40YardDashInfo {

  ArrayList<FootballPlayer02> players = new ArrayList<FootballPlayer02>();

  public void addFootballPlayer(FootballPlayer02 player){
    players.add(player);
  }
  
  public void printPlayerInfo() {

    double avg40YdTime = 0.0;

    System.out.printf("%-15s %15s", "Name", "Avg 40 Time\n");

    // Print dashes under titles
    for (int i = 0; i < 30; i++) {
      System.out.print("_");
    }

    System.out.println();

    for (FootballPlayer player : players) {

      System.out.printf("%-19s", player.getName());

      double total40YdDashTimes = 0.0;
      double[] fortyYardDashTimes = player.get40YardDashTimes();

      for (int i = 0; i < player.get40YardDashTimes().length; i++) {
        total40YdDashTimes += fortyYardDashTimes[i];
      }

      avg40YdTime = total40YdDashTimes / player.get40YardDashTimes().length;
      System.out.printf("%1$.2f", avg40YdTime);
      System.out.println();
    }
  }

  public static void main(String[] args){

    FootballPlayer40YardDashInfo fb40Dash = new FootballPlayer40YardDashInfo();

    // Add Clark Kent for example

    double cKent40YdDashTimes[] = {4.36, 4.39, 4.41};
    FootballPlayer clarkKent = new FootballPlayer("Clark Kent", cKent40YdDashTimes);
    fb40Dash.addFootballPlayer(clarkKent);

    // Add Bruce Wayne for example

    double bWayne40YdDashTimes[] = {4.42, 4.43, 4.49};
    FootballPlayer bruceWayne = new FootballPlayer("Bruce Wayne", bWayne40YdDashTimes);
    fb40Dash.addFootballPlayer(bruceWayne);

    fb40Dash.printPlayerInfo();
  }
}

```
printPlayerInfo() method is quite lengthy and hard to understand. Extracting the codes into 
additional methods will help with understandability here.
```Java
public void printPlayerInfo() {
  printTitles();
  printPlayersWith40Avg();
}

public void printTitles(){
  System.out.printf("%-15s %15s", "Name", "Avg 40 Time\n");
  printDashes('_', 30);
}

public void printDashes(char charToPrint, int howManyTimes){

  for(int i=0;i<howManyTimes; i++){
    System.out.print(charToPrint);System.out.println();
  }
}

public void printPlayersWith40Avg() {

  for(FootballPlayer02 player : players) {

    System.out.printf("%-19s", player.getName());

    double total40YdDashTimes = 0.0;
    double[] fortyYardDashTimes = player.get40YardDashTimes();

    for (int i = 0; i < player.get40YardDashTimes().length; i++){
      total40YdDashTimes+=fortyYardDashTimes[i];
    }

    double avg40YdTime = total40YdDashTimes / player.get40YardDashTimes().length;

    System.out.printf("%1$.2f", avg40YdTime);
  }
}
```
### Calling methods with important local variables:
```Java
double average = 0.0;
double[] dashTimes {4.36, 4.39, 4.41};
for (int i = 0; i < dashTimes.length; i++) { 
  totalDashTimes += dashTimes;
}

average = totalDashtimes / dashTimes.lenght;
```
Instead of the long line of code above, we can substitute it with just 2 lines using an additional method like below:
```Java
double[] dashTimes = {4.36, 4.39, 4.41};
double average = getAvgDashTime(dashTimes);

public static double getAvgDashTime(double[] dashTimes){
		
  double totalDashTimes = 0.0;

  for(int i = 0; i < dashTimes.length; i++){
    totalDashTimes += dashTimes[i];
  }
  
  return totalDashTimes / dashTimes.length;	  
}
```
## When to NOT Extract Methods:
If the code is self-explanatory enough and extracting it into a method does not help with readability.
For example:
```Java
String inTop15 = checkIfInTop15(avg40YdTime) ? " *Top 15\n" : "\n";

public boolean checkIfInTop15(double avg40YdTime) {
  return avg40YdTime < 4.41;
}
```
Compared to this:
```Java
String inTop15 = (avg40YdTime < 4.41) ? " *Top 15\n" : "\n";
```
## When to Get Rid of Temps

```Java
double dashTime = 4.50;
final double avg40YdDash = getAvgDashTime();
String dashGrade = ((dashTime <= avg40YdDash) ? "Good" : "Bad");

System.out.println("That was a " + dashGrage + " time");
```
The temp variable avg40YdDash doesn't add to anything to this code. Instead, we can just include the
method name in the expression below.
```Java
double dashTime = 4.50;
String dashGrade = ((dashTime <= avg40YdDash()) ? "Good" : "Bad");

System.out.println("That was a " + dashGrage + " time");
```
## Replacing Temp Variables with a Query
```Java
double avgDashTime = totalDashTime / totalDashes;

if(avgDashTime > 4.41) {
  System.out.println("Average Time")
}
```
Much better to create an outside method to eliminate temp variables:
```Java
if(avgDashTime() > 4.41) {
  System.out.println("Average Time")
}

double avgDashTime() {
  return totalDashTime / totalDashes;
}
```
