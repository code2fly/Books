# Effective Java - 3rd edition (Deals with Usage not vocabolary or grammar) -

  ## L1 - Creating and Destroying Objects - 
   1. **Item 1** - Consider static factory methods instead of constructors.
      Advantages -
       - methods have name hence better describe purpose/type of object.
       - unlike constructors they are not required to create new objects each time they are invoked.
         - this allows immutable classes to use preconstructed instances.
         - or to cache instances as they are constructed.
       - this helps create instance-controlled classes.
         - this helps create singleton or noninstantiable classes.
         - allows immutable class to gaurantee that no two equal instances exists. (basis of *Flyweight pattern*)
       - *unlike constructor they can return object of any subtype of their return type.*
         - a real life adv would be API can return object without making their class public.(this lends to interface based framework). having inner class as specific impl as in Collections to 
         hide implementation detials. (returned object class may vary from release to release like EnumSet class returns RegularEnumSet and JumboEnumSet instance and may change in future.)
       - class of the returned object need not exist when the class containing the method is written.
         - such flexible static factory method form the basis of *service provider framework* e.g. JDBC API.
         - check more on java.util.ServiceLoader (java service provider framework)

      Disadvantages- 
       - if public/protected constructors are not provided along with it then class cannot be subclassed.(can be encouraging as it promotes composition over inheritence)
       - static methods are hard to find for users. common naming conventions - 
         1. from - type-conversion, take single argument of one type and returns another like Date d = Date.from(instant)
         2. of - aggregation method, takes multiple param and returns instance of this type. 
         Set<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING);
         3. valueOf - BigInteger.valueOf(Integer.MAX_VALUE);
         4. instance or getInstance - 
         5. create or newInstance - Array.newInstance but this guarentees that each call returns new instance.
         6. getType - used if factory method is in different class.
         e.g. FileStore fs = Files.getFileStore(path);
         7. newType - BufferedReader br = Files.newBufferedReader(path);
         8. type - concise alternative to getType and newType. List<Compliant> l = Collections.list()
         
        

      **PS-** Prior to java8 since static methods were not allowd in interface hence by convention static factory for an interface named *Type* were put in non instantiable class named *Types*.


   2. **Item 2** -Consider Builder when faced with many constructor parameters.
     - initially programmers used Telescoping constructor pattern to overcome it.(does not scale well)
     - another option is *JavaBeans pattern* i.e. call parameterless constructor to create object and then call setter methods to set each required parameter or optional parameter of interest.
       - it precludes the possiblity of making classes immutable(hence added effort to ensure thread safety).
     - third approach with safety of Telescoping and readability of Javabeans patterns is builder pattern.
       - instead of creating desired object directly client calls constructor(or static factory) with all required params and gets *builder object*.
       - then client calls setter like methods on builder object to set optional parameter of interest.
       - finally calling the *build()* method to generate object which is generally immutable.
       - builder is typically a *static member class* of the class it builds.

   3. **Item 3** - Enforce a singleton property with a private constructor or an enum type.
   
   
       
   