import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.time.LocalDate;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;

class DailyTask {
    private String name;
    private LocalDate date;
    private LocalTime time;

    public DailyTask(String name, LocalDate date, LocalTime time) {
        this.name = name;
        this.date = date;
        this.time = time;
    }

    public String getName() {
        return this.name;
    }

    public LocalDate getDate() {
        return this.date;
    }

    public LocalTime getTime() {
        return this.time;
    }

    public void display() {
        DateTimeFormatter dateFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd");
        DateTimeFormatter timeFormatter = DateTimeFormatter.ofPattern("HH:mm:ss");
        String var10001 = this.name;
        System.out.println(var10001 + " on " + this.date.format(dateFormatter) + " at " + this.time.format(timeFormatter));
    }

    public void saveToFile() throws IOException {
        String fileName = this.name.replaceAll("\\s+", "") + ".txt";
        BufferedWriter writer = new BufferedWriter(new FileWriter(fileName));
        writer.write("Name: " + this.name + "\n");
        writer.write("Date: " + this.date.toString() + "\n");
        writer.write("Time: " + this.time.toString() + "\n");
        writer.close();
    }
}
import java.time.LocalDate;
import java.time.LocalTime;

class Homework extends DailyTask {
    public Homework(String name, LocalDate date, LocalTime time) {
        super(name, date, time);
    }
}
import java.time.LocalDate;
import java.time.LocalTime;

class Sleep extends DailyTask {
    public Sleep(String name, LocalDate date, LocalTime time) {
        super(name, date, time);
    }
}
import java.time.LocalDate;
import java.time.LocalTime;

class Work extends DailyTask {
    public Work(String name, LocalDate date, LocalTime time) {
        super(name, date, time);
    }
}
import java.time.LocalDate;
import java.time.LocalTime;

class Activity extends DailyTask {
    public Activity(String name, LocalDate date, LocalTime time) {
        super(name, date, time);
 }
 import java.io.IOException;
import java.time.LocalDate;
import java.time.LocalTime;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.Scanner;

public class TaskPlanner {
    private static ArrayList<DailyTask> tasks = new ArrayList();

    public TaskPlanner() {
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter your name: ");
        String name = scanner.nextLine();

        while(true) {
            System.out.print("Enter task type (homework, work, activity, sleep, or quit): ");
            String displayDate = scanner.nextLine().toLowerCase();
            String completeTask;
            if (displayDate.equals("quit")) {
                System.out.print("Enter day to display tasks (yyyy-mm-dd), or type 'all' to display all tasks: ");
                displayDate = scanner.nextLine();
                Iterator var16;
                DailyTask task;
                if (displayDate.equals("all")) {
                    Iterator var13 = tasks.iterator();

                    while(var13.hasNext()) {
                        DailyTask task = (DailyTask)var13.next();
                        task.display();
                    }
                } else {
                    LocalDate date = LocalDate.parse(displayDate);
                    var16 = tasks.iterator();

                    while(var16.hasNext()) {
                        task = (DailyTask)var16.next();
                        if (task.getDate().equals(date)) {
                            task.display();
                        }
                    }
                }

                System.out.print("Enter task name to mark as complete, or type 'none' to skip: ");
                completeTask = scanner.nextLine();
                if (!completeTask.equals("none")) {
                    boolean foundTask = false;
                    Iterator var19 = tasks.iterator();

                    while(var19.hasNext()) {
                        DailyTask task = (DailyTask)var19.next();
                        if (task.getName().equals(completeTask)) {
                            tasks.remove(task);
                            System.out.println("Task marked as complete.");
                            foundTask = true;
                            break;
                        }
                    }

                    if (!foundTask) {
                        System.out.println("Task not found.");
                    }
                }

                scanner.close();

                try {
                    var16 = tasks.iterator();

                    while(var16.hasNext()) {
                        task = (DailyTask)var16.next();
                        task.saveToFile();
                    }
                } catch (IOException var12) {
                    var12.printStackTrace();
                }

                return;
            }

            System.out.print("Enter task name: ");
            completeTask = scanner.nextLine();
            System.out.print("Enter date (yyyy-mm-dd): ");
            String dateString = scanner.nextLine();
            LocalDate date = LocalDate.parse(dateString);
            System.out.print("Enter time (hh:mm:ss): ");
            String timeString = scanner.nextLine();
            LocalTime time = LocalTime.parse(timeString);
            Object task;
            switch (displayDate) {
                case "homework":
                    task = new Homework(completeTask, date, time);
                    break;
                case "work":
                    task = new Work(completeTask, date, time);
                    break;
                case "activity":
                    task = new Activity(completeTask, date, time);
                    break;
                case "sleep":
                    task = new Sleep(completeTask, date, time);
                    break;
                default:
                    System.out.println("Invalid task type. Please try again.");
                    continue;
            }

            if (!isTimeAvailable((DailyTask)task)) {
                System.out.println("Time conflict detected. Please choose another time.");
            } else {
                tasks.add(task);
                System.out.println("Task added successfully!");
                if (task instanceof Sleep) {
                    LocalTime sleepTime = time.minusHours(8L);
                    System.out.println("You should go to sleep at " + sleepTime.toString());
                }
            }
        }
    }

    private static boolean isTimeAvailable(DailyTask var0) {
        // $FF: Couldn't be decompiled
    }
}

