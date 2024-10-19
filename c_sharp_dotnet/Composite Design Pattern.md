 ## The Composite Design Pattern 
is a structural design pattern that allows you to treat individual objects and groups of objects in a uniform manner.
It is used to represent a hierarchical structure of objects, where individual objects and groups of objects are treated in the same way.

The main parts of the Composite Pattern are : 

1- Component (IShape): 
 This is an interface or abstract class that defines the common methods and attributes for all the objects in the hierarchy
 
2- Leaf(Circle,Square,Triangle): 
 This is a class that implements the Component interface or abstract class.
3- Composite (Group): This is a class that also implements the Component interface or abstract class and contains a list of Component objects .

4- Client : 😶 

Examples :

*Folder may have some files when the folder is deleted all of its files must also be deleted because they are treated as part of the folders hierarchy*

*In a graphical application, we have many shapes. We want to group these shapes together to create a composite object that can be treated as a single entity.
For example, if we move the group all the shapes in the group must move togethe.*

![image](https://github.com/user-attachments/assets/c2c1c30f-e44d-42c0-93be-ff0d42601685)

![image](https://github.com/user-attachments/assets/be2e2dbc-73f8-48fa-9264-40caa350344a)

-------
