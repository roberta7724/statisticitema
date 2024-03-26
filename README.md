import java.io.FileNotFoundException;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;
import java.io.File;


class Intervale {
    private double limitaInferioara;
    private double limitaSuperioara;
    private int totalNumbers;
    private int numbersInside;

    public Intervale(double limitaInferioara, double limitaSuperioara) {
        this.limitaInferioara = limitaInferioara;
        this.limitaSuperioara = limitaSuperioara;
        totalNumbers = 0;
        numbersInside = 0;
    }


    public void testNumber(Double number) {
        totalNumbers++;
        if (number >= limitaInferioara && number <= limitaSuperioara) {
            numbersInside++;
        }
    }

    public double getPercentage() {
        return (double) numbersInside / totalNumbers * 100;
    }

    public String toString() {
        return "[" + limitaInferioara + ", " + limitaSuperioara + "]: " + getPercentage() + "%";
    }
}

public class intervale {
    public static void main(String[] args) {
        List<Intervale> intervals = readIntervalsFromFile("intervale.dat");
        List<Double> numbers = readNumbersFromFile("numere.dat");
        calculateStatistics(intervals, numbers);
        writeStatisticsToFile(intervals, "statistica.dat");
    }

    private static List<Intervale> readIntervalsFromFile(String fileName) {
        List<Intervale> intervals = new ArrayList<>();
        try (Scanner scanner = new Scanner(new File(fileName))) {
        	while (scanner.hasNextLine()) {
        	    String line = scanner.nextLine();
        	    String[] parts = line.split("[,\\[\\]]");
        	    if (parts.length >= 3) {
        	        double limitaInferioara = Double.parseDouble(parts[1]);
        	        double limitaSuperioara = Double.parseDouble(parts[2]);
        	        intervals.add(new Intervale(limitaInferioara, limitaSuperioara));
        	    }
        	}

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        return intervals;
    }

    private static List<Double> readNumbersFromFile(String fileName) {
        List<Double> numbers = new ArrayList<>();
        try (Scanner scanner = new Scanner(new File(fileName))) {
            while (scanner.hasNext()) {
                double number = Double.parseDouble(scanner.next());
                numbers.add(number);
            }
        } catch (FileNotFoundException e) {
           
        }
        return numbers;
    }

    private static void calculateStatistics(List<Intervale> intervals, List<Double> numbers) {
        for (Double number : numbers) {
            for (Intervale interval : intervals) {
                interval.testNumber(number);
            }
        }
    }

    private static void writeStatisticsToFile(List<Intervale> intervals, String fileName) {
        try (PrintWriter writer = new PrintWriter(new FileWriter(fileName))) {
            for (Intervale interval : intervals) {
                writer.println(interval.toString());
            }
        } catch (IOException e) {
            
        }
    }
}
