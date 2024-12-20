
###  𝐌𝐕𝐂, 𝐌𝐕𝐏, 𝐌𝐕𝐕𝐌, 𝐕𝐈𝐏𝐄𝐑 

✅ 𝐀𝐫𝐜𝐡𝐢𝐭𝐞𝐜𝐭𝐮𝐫𝐚𝐥 𝐝𝐞𝐬𝐢𝐠𝐧 𝐩𝐚𝐭𝐭𝐞𝐫𝐧𝐬 used to separate concerns in software applications, particularly in UI development. They help in organizing code to make it more maintainable, testable, and scalable.

➡ 𝐌𝐕𝐂 (𝐌𝐨𝐝𝐞𝐥-𝐕𝐢𝐞𝐰-𝐂𝐨𝐧𝐭𝐫𝐨𝐥𝐥𝐞𝐫):

𝐌𝐨𝐝𝐞𝐥: Handles the data and business logic.

𝐕𝐢𝐞𝐰: Manages the presentation layer and displays data.

𝐂𝐨𝐧𝐭𝐫𝐨𝐥𝐥𝐞𝐫: Acts as an intermediary between Model and View, updating the View based on Model changes and handling user input.

➡ 𝐌𝐕𝐏 (𝐌𝐨𝐝𝐞𝐥-𝐕𝐢𝐞𝐰-𝐏𝐫𝐞𝐬𝐞𝐧𝐭𝐞𝐫):

𝐌𝐨𝐝𝐞𝐥: Represents the data and logic.

𝐕𝐢𝐞𝐰: Displays data but has no direct connection to the Model.

𝐏𝐫𝐞𝐬𝐞𝐧𝐭𝐞𝐫: Receives input from the View, interacts with the Model, and updates the View. It drives the logic and flow of the application, with the View being passive.

➡ 𝐌𝐕𝐕𝐌 (𝐌𝐨𝐝𝐞𝐥-𝐕𝐢𝐞𝐰-𝐕𝐢𝐞𝐰𝐌𝐨𝐝𝐞𝐥):

𝐌𝐨𝐝𝐞𝐥: Manages the data and business logic.

𝐕𝐢𝐞𝐰: Displays the data.

𝐕𝐢𝐞𝐰𝐌𝐨𝐝𝐞𝐥: Acts as a binding layer between the View and the Model, exposing data and handling user interactions. It provides a separation of logic for the View.

➡ 𝐕𝐈𝐏𝐄𝐑 (𝐕𝐢𝐞𝐰-𝐈𝐧𝐭𝐞𝐫𝐚𝐜𝐭𝐨𝐫-𝐏𝐫𝐞𝐬𝐞𝐧𝐭𝐞𝐫-𝐄𝐧𝐭𝐢𝐭𝐲-𝐑𝐨𝐮𝐭𝐞𝐫):

𝐕𝐢𝐞𝐰: Displays data and handles user interface.

𝐈𝐧𝐭𝐞𝐫𝐚𝐜𝐭𝐨𝐫: Contains business logic for use cases.

𝐏𝐫𝐞𝐬𝐞𝐧𝐭𝐞𝐫: Coordinates interactions between the View and the Interactor.

𝐄𝐧𝐭𝐢𝐭𝐲: Represents the data models.

𝐑𝐨𝐮𝐭𝐞𝐫: Manages navigation and routing in the application.

![image](https://github.com/user-attachments/assets/762a6bb7-355b-4079-b02b-47645f5965ca)

 
