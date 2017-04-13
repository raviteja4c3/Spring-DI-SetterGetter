# Spring-DI-SetterGetter
DI Using property tag in beans config file &amp; using setter getter methods
------------------------------	
Suppose you have a class 'MainApp' and you want to initilize a bean 'textEditor' [from that call some other class objects anytime] like below, 

public class MainApp {
 public static void main(String[] args) {
      ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
      TextEditor te = (TextEditor) context.getBean("textEditor");  //This will initilize 'textEditor' bean
	  te.spellCheck();   //After initilization we are calling a method. [from that method calling another class method]
   }
} 
   
Then in Beans.xml you just need to add below line of code for bean initilization.
	<bean id = "textEditor" class = "com.tpoint.TextEditor">    

If suppose after 'textEditor' initilization, from that bean method (spellCheck) you want to call other class method (checkSpelling) like below then,

public class TextEditor {
     private SpellChecker spellChecker;
	 
	   public void spellCheck() { //calling other class method inside this method
	      spellChecker.checkSpelling(); }  
	   
	   public void setSpellChecker(SpellChecker spellChecker) {
	      this.spellChecker = spellChecker; }

	   public SpellChecker getSpellChecker() {
	      return spellChecker;	}
}	   

As the object creation of 	'SpellChecker' is needed to call its method (checkSpelling) in 'TextEditor' class then the Beans.xml should be like below:
	<bean id = "textEditor" class = "com.tpoint.TextEditor">
	  	<property name = "spellChecker">
         <bean id = "spellChecker" class = "com.tpoint.SpellChecker"/>
        </property>      
    </bean>
	
Note: The property name in Beans.xml and the variable of class type defined in TextEditor should be same [<property name = "spellChecker">, private SpellChecker spellChecker;]	

# Output: Run MainApp.java
Inside Text Editor Constructor..
Inside SpellCheker Constructor ..
Inside setSpellChecker.
Inside SpellCheker method checkSpelling ...

# Final Note: 
So if you want to call other class methods anytime from your class methods then to get the object of that class, 
1) you need to create property tag under your class bean tag, and under that property tag put the bean tag of class which we want to call. 
2) create variable of class type (which we want object) in your class with same name of 'property' mentioned in Beans.xml, and generate getter, setter methods.
3) that's it! with this variable you can call methods of that class.
