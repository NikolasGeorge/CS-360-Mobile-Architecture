# Inventory Manager: CS 360 Mobile Architecture and Programming

An Android inventory app built in Java for CS 360 at Southern New Hampshire University. The finished app code is in `George_CS360_Project_Three_Option1_Inventory_App.zip`, which also contains the UI I designed in Project Two and used as the baseline for Project Three.

## Reflection

**Briefly summarize the requirements and goals of the app you developed. What user needs was this app designed to address?**

The app was built for a warehouse client that needed to replace manual stock counts with a live record of what is on the shelves. The requirements were a login screen that also creates new accounts, a database with tables for users and inventory items, a grid showing every item, controls to add, remove, and adjust quantities, and a text message alert when any item runs out. The core user need is confidence: warehouse staff need to update stock the moment it changes, and managers and purchasing staff need to know an item is empty before it becomes a missed order.

**What screens and features were necessary to support user needs and produce a user-centered UI for the app? How did your UI designs keep users in mind? Why were your designs successful?**

The app has four screens: login, the inventory grid, an add item form, and an alerts screen. I designed the grid for someone working one-handed on a warehouse floor, so each row puts the decrease, increase, and delete buttons right next to the item, every control is at least 48dp, and the Add Item button sits at the bottom within thumb reach. Out-of-stock rows are tinted red and labeled, so a manager scanning the list sees problems immediately even if text alerts are turned off. Errors appear inline under the field that caused them instead of in pop-ups, and deleting an item asks for confirmation first. The design worked because the most frequent task, changing a quantity, takes a single tap and never sits behind a menu.

**How did you approach the process of coding your app? What techniques or strategies did you use? How could those techniques or strategies be applied in the future?**

I wrote the code against the layouts from Project Two rather than redesigning screens while coding, which kept the work focused. I put all database access in one helper class, so no activity contains SQL, and I kept each class responsible for one job: a password hasher, an SMS alert sender, an adapter that only displays rows, and activities that handle user actions. Passwords are stored only as salted PBKDF2 hashes, and because hashing is deliberately slow, the login check runs on a background thread so the screen never freezes. The grid reloads in `onResume`, so items added on another screen appear as soon as the user returns. Separating data, display, and logic like this will carry over to any app I build, because it makes each part easier to test and change on its own.

**How did you test to ensure your code was functional? Why is this process important, and what did it reveal?**

I tested every requirement in the Android Emulator: logging in with an unknown user, a wrong password, and a duplicate username; creating an account; adding, adjusting, and deleting items; and closing and reopening the app to confirm the data persisted. For SMS, I tested both outcomes. With permission granted, the alert appeared in the emulator's sent messages, and with permission denied, the app kept working with no text and no crash. Testing matters because a feature that only works on the path the developer expects isn't finished. It also caught real problems along the way, such as a duplicated title bar on the login screen that only showed up when I saw the screen running.

**Consider the full app design and development process from initial planning to finalization. Where did you have to innovate to overcome a challenge?**

The biggest challenge came at launch planning. Google Play restricts the SMS permission to apps that act as the device's default messaging app, which means an inventory app that texts users would likely be rejected from a public listing. Instead of dropping the feature, I planned two stages: distribute the app privately to the client's company-managed devices, where SMS alerts work as designed, and replace text messages with standard system notifications for any public release. Earlier, fitting three quantity controls and a delete button into a phone-width row without shrinking them below accessible sizes also took several layout iterations.

**In what specific component of your mobile app were you particularly successful in demonstrating your knowledge, skills, and experience?**

I'm most proud of the SMS alert flow. It explains the alert before asking for permission, checks permission and a saved phone number before every send, fires only on the change that empties a shelf rather than on every tap at zero, and leaves the rest of the app fully functional if the user says no. It brings together permissions, the activity lifecycle, user-centered design, and defensive coding in one feature.
