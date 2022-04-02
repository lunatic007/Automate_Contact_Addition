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
