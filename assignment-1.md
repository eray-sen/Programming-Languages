{\rtf1\ansi\ansicpg1254\cocoartf2907
\cocoatextscaling0\cocoaplatform0{\fonttbl\f0\fnil\fcharset0 Menlo-Regular;\f1\fnil\fcharset0 Menlo-Italic;\f2\fnil\fcharset0 Menlo-Bold;
}
{\colortbl;\red255\green255\blue255;\red0\green0\blue255;\red255\green255\blue254;\red0\green0\blue0;
\red144\green1\blue18;\red0\green0\blue117;\red107\green0\blue1;\red19\green118\blue70;\red15\green112\blue1;
\red191\green28\blue37;}
{\*\expandedcolortbl;;\cssrgb\c0\c0\c100000;\cssrgb\c100000\c100000\c99608;\cssrgb\c0\c0\c0;
\cssrgb\c63922\c8235\c8235;\cssrgb\c0\c6667\c53333;\cssrgb\c50196\c0\c0;\cssrgb\c3529\c52549\c34510;\cssrgb\c0\c50196\c0;
\cssrgb\c80392\c19216\c19216;}
\paperw11900\paperh16840\margl1440\margr1440\vieww11520\viewh8400\viewkind0
\deftab720
\pard\pardeftab720\partightenfactor0

\f0\fs28 \cf2 \cb3 \expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 # Assigment 1\cf0 \cb1 \strokec4 \
\cf2 \cb3 \strokec2 ## Part 1 \'97 Fix the borrow checker\cf0 \cb1 \strokec4 \
\
\
\
\pard\pardeftab720\partightenfactor0

\f1\i \cf0 \cb3 *Each program below is rejected by the compiler. Read the full error message (rustc --explain E0382 etc.), then fix the program without simply adding .clone() everywhere. Explain the fix in a comment.*
\f0\i0 \cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 ---\cf0 \cb1 \strokec4 \
\
\cf2 \cb3 \strokec2 ### (a)\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```rust\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  print_len(s: String) \{ \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "\{\}"\cf0 \strokec4 , s.len()); \}\cb1 \
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  main() \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 let\cf0 \strokec4  s = String::from(\cf5 \strokec5 "hello"\cf0 \strokec4 );\cb1 \
\cb3     print_len(s);\cb1 \
\cb3     \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "\{s\}"\cf0 \strokec4 );\cb1 \
\cb3 \}\cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 * \cf0 \strokec4 The reason for the 
\f2\b **E0382: borrow of moved value**
\f0\b0  error was that print_len() function moved the ownership of variable \cf6 \strokec6 `s`\cf0 \strokec4 . Since the value was moved, the original variable became invalid.\cf7 \strokec7 <br>\cf0 \cb1 \strokec4 \
\cf2 \cb3 \strokec2 * \cf0 \strokec4 If I pass a reference \cf6 \strokec6 `(&s)`\cf0 \strokec4  to the function instead of the value itself, it allows the function to read the data without taking ownership(borrowing).\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3 So 
\f2\b **s**
\f0\b0  remains valid in the main function.\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 #### Fixed Code:\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```rust\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  print_len(s: String) \{ \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "\{\}"\cf0 \strokec4 , s.len()); \} \cb1 \
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  main() \{ \cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 let\cf0 \strokec4  s = String::from(\cf5 \strokec5 "hello"\cf0 \strokec4 ); \cb1 \
\cb3     print_len(&s);\cb1 \
\cb3     \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "\{s\}"\cf0 \strokec4 );\cb1 \
\cb3 \}\cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf0 \cb1 \strokec4 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 ### (b)\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```rust\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  main() \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 let\cf0 \strokec4  \cf2 \strokec2 mut\cf0 \strokec4  v = vec![\cf8 \strokec8 1\cf0 \strokec4 , \cf8 \strokec8 2\cf0 \strokec4 , \cf8 \strokec8 3\cf0 \strokec4 ];\cb1 \
\cb3     \cf2 \strokec2 let\cf0 \strokec4  first = &v[\cf8 \strokec8 0\cf0 \strokec4 ];\cb1 \
\cb3     v.push(\cf8 \strokec8 4\cf0 \strokec4 );\cb1 \
\cb3     \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "first = \{first\}"\cf0 \strokec4 );\cb1 \
\cb3 \}\cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 * \cf0 \strokec4 The reason for the 
\f2\b **E0502: cannot borrow as mutable because it is also borrowed as immutable**
\f0\b0  error was that the program tried to give the variable \cf6 \strokec6 `v`\cf0 \strokec4  both a mutable and an immutable reference. Rust strictly forbids this because \cf6 \strokec6 `v.push(4)`\cf0 \strokec4  might force the vector to resize and move to a new location in memory (reallocation). \cf7 \strokec7 <br>\cf0 \cb1 \strokec4 \
\cf2 \cb3 \strokec2 * \cf0 \strokec4 If you must use a reference, use it before modifying the vector. Once first is printed, its lifespan ends, and the vector becomes safe to modify.\cb1 \
\cf2 \cb3 \strokec2 #### Fixed Code:\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```rust\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  main() \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 let\cf0 \strokec4  \cf2 \strokec2 mut\cf0 \strokec4  v = vec![\cf8 \strokec8 1\cf0 \strokec4 , \cf8 \strokec8 2\cf0 \strokec4 , \cf8 \strokec8 3\cf0 \strokec4 ];\cb1 \
\cb3     \cf2 \strokec2 let\cf0 \strokec4  first = &v[\cf8 \strokec8 0\cf0 \strokec4 ]; \cf9 \strokec9 //immutable borrow starts\cf0 \cb1 \strokec4 \
\cb3     \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "first = \{first\}"\cf0 \strokec4 ); \cf9 \strokec9 // immutable borrow ends here after usage\cf0 \cb1 \strokec4 \
\cb3     v.push(\cf8 \strokec8 4\cf0 \strokec4 ); \cf9 \strokec9 //it is safe to modify the vector now\cf0 \cb1 \strokec4 \
\cb3 \}\cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf0 \cb1 \strokec4 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 ### (f) \cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```rust\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  shout(s: String) -> String \{ s.to_uppercase() \}\cb1 \
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  main() \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 let\cf0 \strokec4  name = String::from(\cf5 \strokec5 "ada"\cf0 \strokec4 );\cb1 \
\cb3     \cf2 \strokec2 let\cf0 \strokec4  r = &name;\cb1 \
\cb3     \cf2 \strokec2 let\cf0 \strokec4  loud = shout(&name);\cb1 \
\cb3     \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "\{r\} \{loud\}"\cf0 \strokec4 );\cb1 \
\cb3 \}\cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 * \cf0 \strokec4 The reason for the 
\f2\b **E0505: cannot move out of `name` because it is borrowed**
\f0\b0  error was that the \cf6 \strokec6 `shout`\cf0 \strokec4  function took the ownership of the \cf6 \strokec6 `name`\cf0 \strokec4  variable while it was immutably borrowed by \cf6 \strokec6 `r`\cf0 \strokec4 . We cannot move a variable before its borrowing ends.\cb1 \
\
\cf2 \cb3 \strokec2 #### Fixed Code:\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```rust\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  shout(s: String) -> String \{ s.to_uppercase() \}\cb1 \
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  main() \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 let\cf0 \strokec4  name = String::from(\cf5 \strokec5 "ada"\cf0 \strokec4 );\cb1 \
\cb3     \cf2 \strokec2 let\cf0 \strokec4  r = &name;\cb1 \
\cb3     \cf2 \strokec2 let\cf0 \strokec4  loud = shout(name);\cb1 \
\cb3     \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "\{r\} \{loud\}"\cf0 \strokec4 );\cb1 \
\cb3 \}\cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf0 \cb1 \strokec4 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 ### (g) \cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```rust\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  main() \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 let\cf0 \strokec4  \cf2 \strokec2 mut\cf0 \strokec4  scores = vec![\cf8 \strokec8 10\cf0 \strokec4 , \cf8 \strokec8 20\cf0 \strokec4 , \cf8 \strokec8 30\cf0 \strokec4 ];\cb1 \
\cb3     \cf2 \strokec2 for\cf0 \strokec4  s \cf2 \strokec2 in\cf0 \strokec4  &scores \{\cb1 \
\cb3         \cf2 \strokec2 if\cf0 \strokec4  *s > \cf8 \strokec8 15\cf0 \strokec4  \{\cb1 \
\cb3             scores.push(*s);\cb1 \
\cb3         \}\cb1 \
\cb3     \}\cb1 \
\cb3     \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "\{scores:?\}"\cf0 \strokec4 );\cb1 \
\cb3 \}\cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 * \cf0 \strokec4 The reason for the 
\f2\b **E0502: cannot borrow `scores` as mutable because it is also borrowed as immutable**
\f0\b0  error was that for loop creates an immutable borrow \cf6 \strokec6 `(&scores)`\cf0 \strokec4  to iterate over the vector. While this borrow is still active inside the loop, the \cf6 \strokec6 `scores.push(*s)`\cf0 \strokec4  line tries to mutably borrow \cf6 \strokec6 `(&mut scores)`\cf0 \strokec4  the exact same vector.\cb1 \
\cf2 \cb3 \strokec2 * \cf0 \strokec4 The way not to get this error is using a temproary array to collect the  values that satisfy the statement and then add it to the main vector:\cb1 \
\cf2 \cb3 \strokec2 #### Fixed Code:\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```rust\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  main() \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 let\cf0 \strokec4  \cf2 \strokec2 mut\cf0 \strokec4  scores = vec![\cf8 \strokec8 10\cf0 \strokec4 , \cf8 \strokec8 20\cf0 \strokec4 , \cf8 \strokec8 30\cf0 \strokec4 ];\cb1 \
\cb3     \cf2 \strokec2 let\cf0 \strokec4  \cf2 \strokec2 mut\cf0 \strokec4  temp_vec = vec![]; \cf9 \strokec9 // temp vector\cf0 \cb1 \strokec4 \
\
\cb3     \cf2 \strokec2 for\cf0 \strokec4  s \cf2 \strokec2 in\cf0 \strokec4  &scores \{\cb1 \
\cb3         \cf2 \strokec2 if\cf0 \strokec4  *s > \cf8 \strokec8 15\cf0 \strokec4  \{\cb1 \
\cb3             temp_vec.push(*s); \cb1 \
\cb3         \}\cb1 \
\cb3     \}\cb1 \
\cb3     scores.extend(temp_vec); \cf9 \strokec9 //updating the scores after borrowing ends.\cf0 \cb1 \strokec4 \
\cb3     \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "\{scores:?\}"\cf0 \strokec4 );\cb1 \
\cb3 \}\cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf0 \cb1 \strokec4 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 ### (h) \cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```rust\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 struct\cf0 \strokec4  Counter \{ value: \cf2 \strokec2 i32\cf0 \strokec4  \}\cb1 \
\cf2 \cb3 \strokec2 impl\cf0 \strokec4  Counter \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 fn\cf0 \strokec4  bump(&\cf2 \strokec2 mut\cf0 \strokec4  \cf2 \strokec2 self\cf0 \strokec4 ) -> \cf2 \strokec2 i32\cf0 \strokec4  \{\cb1 \
\cb3         \cf2 \strokec2 self\cf0 \strokec4 .value += \cf8 \strokec8 1\cf0 \strokec4 ;\cb1 \
\cb3         \cf2 \strokec2 self\cf0 \strokec4 .value\cb1 \
\cb3     \}\cb1 \
\cb3 \}\cb1 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  main() \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 let\cf0 \strokec4  \cf2 \strokec2 mut\cf0 \strokec4  c = Counter \{ value: \cf8 \strokec8 0\cf0 \strokec4  \};\cb1 \
\cb3     \cf2 \strokec2 let\cf0 \strokec4  a = &\cf2 \strokec2 mut\cf0 \strokec4  c;\cb1 \
\cb3     \cf2 \strokec2 let\cf0 \strokec4  b = &\cf2 \strokec2 mut\cf0 \strokec4  c;\cb1 \
\cb3     \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "\{\} \{\}"\cf0 \strokec4 , a.bump(), b.bump());\cb1 \
\cb3 \}\cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3  \cb1 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 * \cf0 \strokec4 The reason for the 
\f2\b **E0499: cannot borrow `c` as mutable more than once at a time**
\f0\b0  is that we are trying to create a second mutable borrow \cf6 \strokec6 `b`\cf0 \strokec4  on the variable \cf6 \strokec6 `c`\cf0 \strokec4  while the first mutable borrow \cf6 \strokec6 `a`\cf0 \strokec4  is still active. But we can only have one active mutable borrow (&mut) at a time for any piece of data to prevent data races.\cb1 \
\cf2 \cb3 \strokec2 * \cf0 \strokec4 The solution is to eliminate the references and use the \cf6 \strokec6 `c`\cf0 \strokec4  variable directly.\cb1 \
\cf2 \cb3 \strokec2 #### Fixed Code: \cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```rust\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 struct\cf0 \strokec4  Counter \{ value: \cf2 \strokec2 i32\cf0 \strokec4  \}\cb1 \
\cf2 \cb3 \strokec2 impl\cf0 \strokec4  Counter \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 fn\cf0 \strokec4  bump(&\cf2 \strokec2 mut\cf0 \strokec4  \cf2 \strokec2 self\cf0 \strokec4 ) -> \cf2 \strokec2 i32\cf0 \strokec4  \{\cb1 \
\cb3         \cf2 \strokec2 self\cf0 \strokec4 .value += \cf8 \strokec8 1\cf0 \strokec4 ;\cb1 \
\cb3         \cf2 \strokec2 self\cf0 \strokec4 .value\cb1 \
\cb3     \}\cb1 \
\cb3 \}\cb1 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  main() \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 let\cf0 \strokec4  \cf2 \strokec2 mut\cf0 \strokec4  c = Counter \{ value: \cf8 \strokec8 0\cf0 \strokec4  \};\cb1 \
\cb3     \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "\{\} \{\}"\cf0 \strokec4 , c.bump(), c.bump());\cb1 \
\cb3 \}\cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf0 \cb1 \strokec4 \
\
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 ## Part 2 \'97 Strings and slices\cf0 \cb1 \strokec4 \
\
\pard\pardeftab720\partightenfactor0

\f1\i \cf0 \cb3 *String is an owned, growable, heap-allocated UTF-8 buffer. &str is a borrowed view into UTF-8 data (part of a String, or a literal). Functions that only read text should take &str. Then they accept both &String and literals.*
\f0\i0 \cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 ### 1)\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0

\f1\i \cf0 \cb3 *fn first_word(s: &str) -> &str \'97 the text up to the first space, or the whole string. first_word("hello world") == "hello", first_word("single") == "single"*
\f0\i0 \cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```rust\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  first_word(s: &\cf2 \strokec2 str\cf0 \strokec4 ) -> &\cf2 \strokec2 str\cf0 \strokec4  \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     s.split(\cf10 \strokec10 '\cf0 \strokec4  \cf10 \strokec10 '\cf0 \strokec4 ).next().unwrap_or(s)\cb1 \
\cb3 \}\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  main() \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "\{\}"\cf0 \strokec4 , first_word(\cf5 \strokec5 "hello world"\cf0 \strokec4 )); \cb1 \
\cb3     \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "\{\}"\cf0 \strokec4 , first_word(\cf5 \strokec5 "single"\cf0 \strokec4 ));      \cb1 \
\cb3 \}\cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 * \cf0 \strokec4 1.  s.split(' '): Splits the string eat the first space into pieces as array elements.\cb1 \
\cf2 \cb3 \strokec2 * \cf0 \strokec4 2. .next(): Takes the first piece of the elements.\cb1 \
\cf2 \cb3 \strokec2 * \cf0 \strokec4 3. .unwrap_or(s): In case the string is without a space, it returns the original string.\cb1 \
\
\cf2 \cb3 \strokec2 ### 2)\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0

\f1\i \cf0 \cb3 *fn count_vowels(s: &str) -> usize \'97 count aeiou, ignoring case. Use s.chars().*
\f0\i0 \cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```rust\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  count_vowels(s: &\cf2 \strokec2 str\cf0 \strokec4 ) -> usize \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 let\cf0 \strokec4  \cf2 \strokec2 mut\cf0 \strokec4  count = \cf8 \strokec8 0\cf0 \strokec4 ; \cf9 \strokec9 //counter\cf0 \cb1 \strokec4 \
\
\cb3     \cf9 \strokec9 // s.chars() -> Break the text down into its individual letters.\cf0 \cb1 \strokec4 \
\cb3     \cf2 \strokec2 for\cf0 \strokec4  c \cf2 \strokec2 in\cf0 \strokec4  s.chars() \{\cb1 \
\cb3         \cf9 \strokec9 // .to_ascii_lowercase() -> (A -> a, E->e etc)\cf0 \cb1 \strokec4 \
\cb3         \cf2 \strokec2 let\cf0 \strokec4  lowercase_char = c.to_ascii_lowercase();\cb1 \
\
\cb3         \cf2 \strokec2 if\cf0 \strokec4  lowercase_char == \cf5 \strokec5 'a'\cf0 \strokec4  || lowercase_char == \cf5 \strokec5 'e'\cf0 \strokec4  || lowercase_char == \cf5 \strokec5 'i'\cf0 \strokec4  || lowercase_char == \cf5 \strokec5 'o'\cf0 \strokec4  || lowercase_char == \cf5 \strokec5 'u'\cf0 \strokec4  \{\cb1 \
\cb3             count += \cf8 \strokec8 1\cf0 \strokec4 ;\cb1 \
\cb3         \}\cb1 \
\cb3     \}\cb1 \
\cb3     count \cf9 \strokec9 // returning "count" as the output.\cf0 \cb1 \strokec4 \
\cb3 \}\cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf0 \cb1 \strokec4 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 ### 3)\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0

\f1\i \cf0 \cb3 *fn reverse_words(s: &str) -> String \'97 "the quick brown fox" \uc0\u8594  "fox brown quick the". Use split_whitespace.*
\f0\i0 \cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```rust\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  reverse_words(s: &\cf2 \strokec2 str\cf0 \strokec4 ) -> String \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 let\cf0 \strokec4  \cf2 \strokec2 mut\cf0 \strokec4  words: Vec<&\cf2 \strokec2 str\cf0 \strokec4 > = s.split_whitespace().collect();\cb1 \
\cb3     words.reverse(); \cf9 \strokec9 //reversing\cf0 \cb1 \strokec4 \
\
\cb3     words.join(\cf5 \strokec5 " "\cf0 \strokec4 ) \cf9 \strokec9 //combining words into a single string of characters with spaces between the words\cf0 \cb1 \strokec4 \
\cb3 \}\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 ### 4)\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0

\f1\i \cf0 \cb3 *fn capitalize(s: &str) -> String \'97 upper-case the first character: "rust" \uc0\u8594  "Rust", "" \u8594  "".*
\f0\i0 \cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```rust\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  capitalize(s: &\cf2 \strokec2 str\cf0 \strokec4 ) -> String \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf9 \strokec9 // if the string is empty, return an empty String\cf0 \cb1 \strokec4 \
\cb3     \cf2 \strokec2 if\cf0 \strokec4  s.is_empty() \{\cb1 \
\cb3         \cf2 \strokec2 return\cf0 \strokec4  String::new();\cb1 \
\cb3     \}\cb1 \
\
\cb3     \cf9 \strokec9 // getting the first character and convert it to uppercase\cf0 \cb1 \strokec4 \
\cb3     \cf2 \strokec2 let\cf0 \strokec4  first = s.chars().next().unwrap().to_uppercase();\cb1 \
\cb3     \cb1 \
\cb3     \cf9 \strokec9 // taking a slice of the remaining characters from index 1 to the end\cf0 \cb1 \strokec4 \
\cb3     \cf2 \strokec2 let\cf0 \strokec4  remainder = &s[\cf8 \strokec8 1\cf0 \strokec4 ..]; \cb1 \
\
\cb3     \cf2 \strokec2 format!\cf0 \strokec4 (\cf5 \strokec5 "\{\}\{\}"\cf0 \strokec4 , first, remainder) \cf9 \strokec9 //combining them\cf0 \cb1 \strokec4 \
\cb3 \}\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  main() \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "\{\}"\cf0 \strokec4 , capitalize(\cf5 \strokec5 "rust"\cf0 \strokec4 )); \cb1 \
\cb3     \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "\{\}"\cf0 \strokec4 , capitalize(\cf5 \strokec5 ""\cf0 \strokec4 ));      \cb1 \
\cb3 \}\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 ### 5)\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0

\f1\i \cf0 \cb3 *fn is_palindrome(s: &str) -> bool \'97 ignore case and anything that is not alphanumeric: is_palindrome("A man, a plan, a canal: Panama") is true*
\f0\i0 \cb1 \
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```rust\cf0 \cb1 \strokec4 \
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  is_palindrome(s: &\cf2 \strokec2 str\cf0 \strokec4 ) -> \cf2 \strokec2 bool\cf0 \strokec4  \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 let\cf0 \strokec4  cleaned: String = s.chars()\cb1 \
\cb3         .filter(|c| c.is_alphanumeric())   \cf9 \strokec9 // only letters or digits \cf0 \cb1 \strokec4 \
\cb3         .map(|c| c.to_ascii_lowercase())   \cf9 \strokec9 // converting all to lowercase\cf0 \cb1 \strokec4 \
\cb3         .collect();                          \cb1 \
\
\cb3     cleaned == cleaned.chars().rev().collect::<String>()\cb1 \
\cb3 \}\cb1 \
\
\pard\pardeftab720\partightenfactor0
\cf2 \cb3 \strokec2 fn\cf0 \strokec4  main() \{\cb1 \
\pard\pardeftab720\partightenfactor0
\cf0 \cb3     \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "\{\}"\cf0 \strokec4 , is_palindrome(\cf5 \strokec5 "A man, a plan, a canal: Panama"\cf0 \strokec4 )); \cf9 \strokec9 // true\cf0 \cb1 \strokec4 \
\cb3     \cf2 \strokec2 println!\cf0 \strokec4 (\cf5 \strokec5 "\{\}"\cf0 \strokec4 , is_palindrome(\cf5 \strokec5 "race a car"\cf0 \strokec4 ));  \cf9 \strokec9 // false\cf0 \cb1 \strokec4 \
\cb3 \}\cb1 \
\
\
\pard\pardeftab720\partightenfactor0
\cf5 \cb3 \strokec5 ```\cf0 \cb1 \strokec4 \
\
\
}