package jdbc;
import java.sql.*;
import java.util.Scanner;

public class HospitalManagementSystem {

    public static void main(String[] args) {

        String url = "jdbc:mysql://localhost:3306/hospital";  // database name: hospital
        String user = "root";       // your MySQL username
        String pass = "@shree101";  // your MySQL password

        Scanner sc = new Scanner(System.in);

        try {
            // 1️⃣ Load and register driver
            Class.forName("com.mysql.cj.jdbc.Driver");

            // 2️⃣ Establish connection
            Connection con = DriverManager.getConnection(url, user, pass);
            System.out.println("✅ Connected to Hospital Database successfully! - HospitalManagementSystem.java:21");

            while (true) {
                System.out.println("\n=== 🏥 Hospital Management Menu === - HospitalManagementSystem.java:24");
                System.out.println("1. Add Patient - HospitalManagementSystem.java:25");
                System.out.println("2. Display All Patients - HospitalManagementSystem.java:26");
                System.out.println("3. Update Patient Details - HospitalManagementSystem.java:27");
                System.out.println("4. Delete Patient - HospitalManagementSystem.java:28");
                System.out.println("5. Exit - HospitalManagementSystem.java:29");
                System.out.print("Enter your choice: - HospitalManagementSystem.java:30");
                int choice = sc.nextInt();

                switch (choice) {

                    case 1:
                        // ➕ INSERT Operation
                        System.out.print("Enter Patient ID: - HospitalManagementSystem.java:37");
                        int pid = sc.nextInt();
                        sc.nextLine(); // consume newline
                        System.out.print("Enter Patient Name: - HospitalManagementSystem.java:40");
                        String pname = sc.nextLine();
                        System.out.print("Enter Age: - HospitalManagementSystem.java:42");
                        int age = sc.nextInt();
                        sc.nextLine();
                        System.out.print("Enter Disease: - HospitalManagementSystem.java:45");
                        String disease = sc.nextLine();

                        String insertQuery = "INSERT INTO patient VALUES (?, ?, ?, ?)";
                        PreparedStatement psInsert = con.prepareStatement(insertQuery);
                        psInsert.setInt(1, pid);
                        psInsert.setString(2, pname);
                        psInsert.setInt(3, age);
                        psInsert.setString(4, disease);

                        int rowsInserted = psInsert.executeUpdate();
                        System.out.println(rowsInserted + "patient record inserted successfully! - HospitalManagementSystem.java:56");
                        psInsert.close();
                        break;

                    case 2:
                        // 📋 DISPLAY Operation
                        String selectQuery = "SELECT * FROM patient";
                        Statement stmt = con.createStatement();
                        ResultSet rs = stmt.executeQuery(selectQuery);

                        System.out.println("\nID\tName\tAge\tDisease - HospitalManagementSystem.java:66");
                        System.out.println("");
                        while (rs.next()) {
                            System.out.println(
                                rs.getInt("id") + "\t" +
                                rs.getString("name") + "\t" +
                                rs.getInt("age") + "\t" +
                                rs.getString("disease")
                            );
                        }
                        rs.close();
                        stmt.close();
                        break;

                    case 3:
                        // ✏ UPDATE Operation
                        System.out.print("Enter Patient ID to update: - HospitalManagementSystem.java:82");
                        int uid = sc.nextInt();
                        sc.nextLine();
                        System.out.print("Enter new Name: - HospitalManagementSystem.java:85");
                        String newName = sc.nextLine();
                        System.out.print("Enter new Age: - HospitalManagementSystem.java:87");
                        int newAge = sc.nextInt();
                        sc.nextLine();
                        System.out.print("Enter new Disease: - HospitalManagementSystem.java:90");
                        String newDisease = sc.nextLine();

                        String updateQuery = "UPDATE patient SET name=?, age=?, disease=? WHERE id=?";
                        PreparedStatement psUpdate = con.prepareStatement(updateQuery);
                        psUpdate.setString(1, newName);
                        psUpdate.setInt(2, newAge);
                        psUpdate.setString(3, newDisease);
                        psUpdate.setInt(4, uid);

                        int rowsUpdated = psUpdate.executeUpdate();
                        System.out.println(rowsUpdated + "patient record updated successfully! - HospitalManagementSystem.java:101");
                        psUpdate.close();
                        break;

                    case 4:
                        // ❌ DELETE Operation
                        System.out.print("Enter Patient ID to delete: - HospitalManagementSystem.java:107");
                        int did = sc.nextInt();

                        String deleteQuery = "DELETE FROM patient WHERE id=?";
                        PreparedStatement psDelete = con.prepareStatement(deleteQuery);
                        psDelete.setInt(1, did);

                        int rowsDeleted = psDelete.executeUpdate();
                        System.out.println(rowsDeleted + "patient record deleted successfully! - HospitalManagementSystem.java:115");
                        psDelete.close();
                        break;

                    case 5:
                        // 🚪 EXIT
                        System.out.println("Exiting... Thank you for using Hospital Management System! - HospitalManagementSystem.java:121");
                        con.close();
                        sc.close();
                        System.exit(0);

                    default:
                        System.out.println("❌ Invalid choice! Please try again. - HospitalManagementSystem.java:127");
                }
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

