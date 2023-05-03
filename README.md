Download Link: https://assignmentchef.com/product/solved-cs30-project-2-shared-pointers
<br>
<strong><em><u>Homework 2:  Shared Pointers</u></em></strong>

In this homework, you will make your own smart pointer type. You will write the following class to develop a referenced counted smart pointer.  You will also implement a few other member functions to resemble the functionality of an ordinary raw pointer. Basically, this is a problem of designing a single, non-trivial class and overloading a few pointer related operators. You may not use any of the STLs except for stdexcept (“So what you’re saying is ‘exceptions is the only exception’?” – Former CS 30 Student) to do this, i.e., please don’t try and use a shared_ptr to implement the class. You can start with this code, and you may add other member functions, if you want.

template &lt;typename T&gt; class smart_ptr { public:

smart_ptr();

// Create a smart_ptr that is initialized to nullptr. The   // reference count should be initialized to nullptr.

explicit smart_ptr(T* raw_ptr);

// Create a smart_ptr that is initialized to raw_ptr. The

// reference count should be one.  Make sure it points to  // the same pointer as the raw_ptr.

smart_ptr(const smart_ptr&amp; rhs);

// Copy construct a pointer from rhs. The reference count   // should be incremented by one.

smart_ptr(smart_ptr&amp;&amp; rhs);

// Move construct a pointer from rhs.

smart_ptr&amp; operator=(const smart_ptr&amp; rhs);

// This assignment should make a shallow copy of the

// right-hand side’s pointer data. The reference count

// should be incremented as appropriate.

smart_ptr&amp; operator=(smart_ptr&amp;&amp; rhs);

// This move assignment should steal the right-hand side’s

// pointer data.




bool clone();

// If the smart_ptr is either nullptr or has a reference

// count of one, this function will do nothing and return

// false. Otherwise, the referred to object’s reference

// count will be decreased and a new deep copy of the

// object will be created. This new copy will be the

// object that this smart_ptr points and its reference

// count will be one.

int ref_count() const;

// Returns the reference count of the pointed to data.




T&amp; operator*();

// The dereference operator shall return a reference to   // the referred object. Throws null_ptr_exception on

// invalid access.

T* operator-&gt;();

// The arrow operator shall return the pointer ptr_.   // Throws null_ptr_exception on invalid access.




~smart_ptr();          // deallocate all dynamic memory

private:

T* ptr_;               // pointer to the referred object     int* ref_;             // pointer to a reference count

};




Here is an example of how the above class might work.

int* p { new int { 42 } }; smart_ptr&lt;int&gt; sp1 { p };




// Ref Count is 1 cout &lt;&lt; “Ref count is ” &lt;&lt; sp1.ref_count() &lt;&lt; endl;

{    smart_ptr&lt;int&gt; sp2 { sp1 };    // Ref Count is 2    cout &lt;&lt; “Ref count is ” &lt;&lt; sp1.ref_count() &lt;&lt; endl;

// Ref Count is 2    cout &lt;&lt; “Ref count is ” &lt;&lt; sp2.ref_count() &lt;&lt; endl;

}




// Ref Count is 1 cout &lt;&lt; “Ref count is ” &lt;&lt; sp1.ref_count() &lt;&lt; endl;

smart_ptr&lt;int&gt; sp3;




// Ref Count is 0

cout &lt;&lt; “Ref count is ” &lt;&lt; sp3.ref_count() &lt;&lt; endl;

sp3 = sp1;




// Ref Count is 2 cout &lt;&lt; “Ref count is ” &lt;&lt; sp1.ref_count() &lt;&lt; endl; // Ref Count is 2 cout &lt;&lt; “Ref count is ” &lt;&lt; sp3.ref_count() &lt;&lt; endl;

smart_ptr&lt;int&gt; sp4 = std::move(sp1);

cout &lt;&lt; *sp4 &lt;&lt; ” ” &lt;&lt; *sp3 &lt;&lt; endl;        // prints 42 42 cout &lt;&lt; *sp1 &lt;&lt; endl;   // throws null_ptr_exception




The arrow operator will only compile and work if the referred object is a class type:

struct Point { int x = 2; int y = -5; };

int main ( )

{    smart_ptr&lt;Point&gt; sp { new Point };    cout &lt;&lt; sp-&gt;x &lt;&lt; ” ” &lt;&lt; sp-&gt;y &lt;&lt; endl;   // prints 2 -5    return 0;

}




Here is an example of the clone member function:

smart_ptr&lt;double&gt; dsp1 { new double {3.14} }; smart_ptr&lt;double&gt; dsp2, dsp3;

dsp3 = dsp2 = dsp1;

cout &lt;&lt; dsp1.ref_count() &lt;&lt; ” ” &lt;&lt; dsp2.ref_count() &lt;&lt; ” ”

&lt;&lt; dsp3.ref_count() &lt;&lt; endl; // prints 3 3 3

// prints 3.14 3.14 3.14 cout &lt;&lt; *dsp1 &lt;&lt; ” ” &lt;&lt; *dsp2 &lt;&lt; ” ” &lt;&lt; *dsp3 &lt;&lt; endl;

dsp1.clone();        // returns true

cout &lt;&lt; dsp1.ref_count() &lt;&lt; ” ” &lt;&lt; dsp2.ref_count() &lt;&lt; ” ”

&lt;&lt; dsp3.ref_count() &lt;&lt; endl; // prints 1 2 2

// prints 3.14 3.14 3.14 cout &lt;&lt; *dsp1 &lt;&lt; ” ” &lt;&lt; *dsp2 &lt;&lt; ” ” &lt;&lt; *dsp3 &lt;&lt; endl; <strong><u>Requirements/Hints</u></strong>

Here are some of the requirements for writing the class. Test the implemented member functions to verify their basic functionality, and also how much of the desired semantics that is achieved by this implementation, and also if there are any undesired effects.

<ol>

 <li>The null_ptr_exception is an exception that you will define, it should be derived from an STL exception.</li>

 <li>Label the above member functions as noexcept where appropriate.</li>

 <li>You may add additional member functions, if you’d like.</li>

 <li>Recognize that move constructors/assignments result in the reference count remaining the same, hence there’s no need to change it.</li>

 <li>Think about writing as exception safe code as possible. What type of guarantees should each member function have?</li>

</ol>




<h1>Extensions</h1>




If you have time, see if you can research how to get the following to work for a smart_ptr&lt;X&gt; sp. These require additional functions:

<ol>

 <li>Direct null pointer test: if (sp)</li>

 <li>Negated direct null pointer test: if (!sp)</li>

 <li>Explicit equality test for null pointers, sp == nullptr , nullptr == sp</li>

 <li>Explicit inequality test for null pointers, sp != nullptr , nullptr != sp</li>

 <li>The above should be true without allowing delete sp to compile</li>

 <li>Initialization to nullptr: smart_ptr&lt;X&gt; sp = nullptr</li>

</ol>


