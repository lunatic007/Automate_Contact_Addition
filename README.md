# Automate_Contact_Addition
Add contacts to device through java-adb code through automation

Steps to add contacts:
- Run command to check whether there is or more contacts already saved in device (it was my requirement)
- If contacts_count < 10 : run your contact addition code ? continou to further code
- Contact < 10:
- Store all contacts in with name & number in a string array. Format eg: "Test1|8527801638", "Test2|8527801658", ....etc
- Run command to clear already stored contacts in device, if any
- Run a for loop to iterate contacts in string array. Split contact name & number using regex "|".
- Run command to add contact to device. It will open contact saving page and prefill name & number field.
- Now we need to tap on save button by using xpath of the loacator for save button.
- Code End


Code:

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;

public class addContacts {
    public void Add_ContactsToDevice() {
        String contactCount = null;
        try {
            contactCount = runCommand("adb shell content query --uri content://contacts/people/|wc -l");
            System.out.println("contactCount : " + contactCount);
            System.out.println("Total Contact on device: " + contactCount.trim());
        } catch (IOException e) {
            e.printStackTrace();
        }
        if (Integer.parseInt(contactCount.trim()) < 10) {
            System.out.println("<-----Adding contacts in device for performing test---->");
//        String[] contactList = new String[]{"Test1|8527801638",""};
            ArrayList<String> contactList = new ArrayList<String>(Arrays.asList("Test1|8527801638", "Test2|8527801658", "Test3|8527801888", "Test4|8527801678", "Test5|8527801688", "Test6|8527801698", "Test7|8527801610", "Test8|8527801620", "Test9|8527801630", "Test10|8527801898"));
            try {
                ProcessBuilder p1 = new ProcessBuilder("adb", "shell", "pm", "clear", "com.android.providers.contacts"); //clear already added contacts
                p1.start();
                gotoSleep(2000);
                for (String contact : contactList) {
                    try {
                        String cName = contact.split("\\|")[0];
                        String cPhone = contact.split("\\|")[1];
                        System.out.println("Name: " + cName + " & " + "Phone: " + cPhone);
                        ProcessBuilder p2 = new ProcessBuilder("adb", "shell", "am", "start", "-a", "android.intent.action.INSERT", "-t", "vnd.android.cursor.dir/contact", "-e", "name", "" + cName + "", "-e", "phone", "" + cPhone + "");
                        p2.start();
                        gotoSleep(4000);
                        //Code to tap on save button depends on framework to framework, please write according to your own framework
                        //Tap on Save button
                        //Press return back button of device 
                        gotoSleep(2000);
                    } catch (Exception e) {
                    }
                }
//            ProcessBuilder p2 = new ProcessBuilder("adb","shell","am","start","-a","android.intent.action.INSERT","-t","vnd.android.cursor.dir/contact","-e","name","'Test1'","-e","phone","8527801638");
//            p2.start();
//            ActionHelper.gotoSleep(10000);
//            ActionHelper.click(By.xpath("//*[@text='Save' or @text='SAVE']"));
                gotoSleep(3000);
            } catch (Exception e) {
                e.printStackTrace();
            }
            System.out.println("<-----Done addding contact----->");
        }
        else {
            System.out.println("<---More Than 10 contacts already added, no need to add contact--->");
        }
    }

    public static String runCommand(String command) throws IOException {
        Process process = Runtime.getRuntime().exec(command);
        BufferedReader in = new BufferedReader(new InputStreamReader(process.getInputStream()));

        String line;
        while ((line = in.readLine()) != null) {
            System.out.println(line);
            return line;
        }
        return line;
    }

    public static void gotoSleep(int milliSeconds) {
        try {
            Thread.sleep((long)milliSeconds);
        } catch (InterruptedException var2) {
            var2.printStackTrace();
        }

    }
}



