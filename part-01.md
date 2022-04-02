# Replace Constructors with Creation Methods

```Java
public class FootballPlayer {

  private double passerRating; // Specific to QBs
  private int rushingYards; // Specific to RBs & QBs
  private int receivingYards; // Specific to RBs & WRs
  private int totalTackles; // Specific to DEF
  private int interceptions; // Specific to DEF
  private int fieldGoals; // Specific to Kickers
  private double avgPunt; // Specific to Punters
  private double avgKickoffReturn; // Specific to Special Teams
  private double avgPuntReturn; // Specific to Special Teams
  
  FootballPlayer(double passerRating, int rushingYards) {
    this.passerRating = passerRating;
    this.rushingYards = rushingYards;
  }

  FootballPlayer(int rushingYards) {
    this.rushingYards = rushingYards;
  }

  FootballPlayer(int receivingYards) {
    this.receivingYards = receivingYards;
  }
}
```
Error: Two constructors above have the same amount of attribute and attribute type.
<br>
Instead of the constructors above, Creation Methods help solve this problem.

```Java
private FootballPlayer(double passerRating, int rushingYards, int receivingYards,
    int totalTackles, int interceptions, int fieldGoals, double avgPunt, double avgKickoffReturn,
    double avgPuntReturn) {

  this.passerRating = passerRating;
  this.rushingYards = rushingYards;
  this.receivingYards = receivingYards;
  this.totalTackles = totalTackles;
  this.interceptions = interceptions;
  this.fieldGoals = fieldGoals;
  this.avgPunt = avgPunt;
  this.avgKickoffReturn = avgKickoffReturn;
  this.avgPuntReturn = avgPuntReturn;
}

public static FootballPlayer createQB(double passerRating, int rushingYards) {

  return new FootballPlayer(passerRating, rushingYards, 0, 0, 0, 0, 0.0, 0.0, 0.0);
}

public double getPasserRating() {
  return passerRating;
}

public static void main(String[] args) {
  FootballPlayer aaronRodgers = FootballPlayer.createQB(108.0, 259);

  System.out.println("Aaron Rodgers Passer Rating: " + aaronRodgers.getPasserRating());
}
```

# Avoid Duplication & Chain Constructors

```Java
public class FootballPlayer2 {

  private String playerName = "";
  private String college = "";
  private double fortyYardDash = 0.0;
  private int repsBenchPress = 0;
  private double sixtyYardDash = 0.0;

  public String getPlayerName() {
    return playerName;
  }

  public String getCollege() {
    return college;
  }

  public double get40YdDash() {
    return fortyYardDash;
  }

  public int getRepsBenchPress() {
    return repsBenchPress;
  }

  public double get60YdDash() {
    return sixtyYardDash;
  }
  
  public FootballPlayer2(String playerName, String college,
      double fortyYardDash, double sixtyYardDash) {

    this.playerName = playerName;
    this.college = college;
    this.fortyYardDash = fortyYardDash;
    this.sixtyYardDash = sixtyYardDash;

  }

  public FootballPlayer2(String playerName, String college,
      double fortyYardDash, int repsBenchPress) {

    this.playerName = playerName;
    this.college = college;
    this.fortyYardDash = fortyYardDash;
    this.repsBenchPress = repsBenchPress;

  }

  public FootballPlayer2(String playerName, String college,
      double fortyYardDash, int repsBenchPress, double sixtyYardDash) {

    this.playerName = playerName;
    this.college = college;
    this.fortyYardDash = fortyYardDash;
    this.repsBenchPress = repsBenchPress;
    this.sixtyYardDash = sixtyYardDash;

  } 
```

Chaining constructors work much better here:

```Java
public FootballPlayer2(String playerName, String college, double fortyYardDash, int repsBenchPress,
      double sixtyYardDash) {

    this.playerName = playerName;
    this.college = college;
    this.fortyYardDash = fortyYardDash;
    this.repsBenchPress = repsBenchPress;
    this.sixtyYardDash = sixtyYardDash;
  }

  public FootballPlayer2(String playerName, String college, double fortyYardDash,
      int repsBenchPress) {

    this(playerName, college, fortyYardDash, repsBenchPress, 0.0);
  }

  public FootballPlayer2(String playerName, String college, double fortyYardDash,
      double sixtyYardDash) {

    this(playerName, college, fortyYardDash, 0, sixtyYardDash);
  }
```






