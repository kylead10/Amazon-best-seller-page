

- utilize spread operator (...) - copy values
- Spread Operator
- [JS Nuggets - S
#### Events - Fundamentals

- Vanilla JS

```js
const btn = document.getElementById('btn');

btn.addEventListener('click', function (e) {
  // access event object
  // do something when event fires
});
```

- similar approach
- element, event, function
- again camelCase

```js
const EventExamples = () => {
  const handleButtonClick = () => {
    alert('handle button click');
  };
  return (
    <section>
      <button onClick={handleButtonClick}>click me</button>
    </section>
  );
};
```

- [React Events](https://reactjs.org/docs/events.html)
- no need to memorize them(idea is the same)
- most common
  - onClick (click events)
  - onSubmit (submit form )
  - onChange (input change )

```js
function BookList() {
  return (
    <section className='booklist'>
      <EventExamples />
      {books.map((book) => {
        return <Book {...book} key={book.id} />;
      })}
    </section>
  );
}

const EventExamples = () => {
  const handleFormInput = () => {
    console.log('handle form input');
  };
  const handleButtonClick = () => {
    alert('handle button click');
  };
  return (
    <section>
      <form>
        <h2>Typical Form</h2>
        <input
          type='text'
          name='example'
          onChange={handleFormInput}
          style={{ margin: '1rem 0' }}
        />
      </form>
      <button onClick={handleButtonClick}>click me</button>
    </section>
  );
};
```

#### Event Object and Form Submission

```js
const EventExamples = () => {
  const handleFormInput = (e) => {
    console.log(e);
    // e.target - element
    console.log(`Input Name : ${e.target.name}`);
    console.log(`Input Value : ${e.target.value}`);
    // console.log('handle form input');
  };
  const handleButtonClick = () => {
    alert('handle button click');
  };
  const handleFormSubmission = (e) => {
    e.preventDefault();
    console.log('form submitted');
  };
  return (
    <section>
      {/* add onSubmit Event Handler */}
      <form onSubmit={handleFormSubmission}>
        <h2>Typical Form</h2>
        <input
          type='text'
          name='example'
          onChange={handleFormInput}
          style={{ margin: '1rem 0' }}
        />
        {/* add button with type='submit' */}
        <button type='submit'>submit form</button>
      </form>
      <button onClick={handleButtonClick}>click me</button>
    </section>
  );
};
```

- alternative approach

```js
<button type='submit' onClick={handleFormSubmission}>
  submit form
</button>
```

#### Mind Grenade

- alternative approach
- pass anonymous function (in this case arrow function)
- one liner - less code

```js
const EventExamples = () => {
  return (
    <section>
      <button onClick={() => console.log('hello there')}>click me</button>
    </section>
  );
};
```

- also can access event object

```js
const EventExamples = () => {
  return (
    <section>
      <form>
        <h2>Typical Form</h2>
        <input
          type='text'
          name='example'
          onChange={(e) => console.log(e.target.value)}
          style={{ margin: '1rem 0' }}
        />
      </form>
      <button onClick={() => console.log('you clicked me')}>click me</button>
    </section>
  );
};
```

#### Mind Grenade #2

- remove EventsExamples
- components are independent by default

```js
function BookList() {
  return (
    <section className='booklist'>
      {books.map((book) => {
        return <Book {...book} key={book.id} />;
      })}
    </section>
  );
}

const Book = (props) => {
  const { img, title, author } = props;
  const displayTitle = () => {
    console.log(title);
  };

  return (
    <article className='book'>
      <img src={img} alt={title} />
      <h2>{title}</h2>
      <button onClick={displayTitle}>display title</button>
      <h4>{author} </h4>
    </article>
  );
};
```

- remove button

#### Prop Drilling

- react data flow - can only pass props down
- alternatives Context API, redux, other state libraries

```js
function BookList() {
  const someValue = 'shakeAndBake';
  const displayValue = () => {
    console.log(someValue);
  };
  return (
    <section className='booklist'>
      {books.map((book) => {
        return <Book {...book} key={book.id} displayValue={displayValue} />;
      })}
    </section>
  );
}

const Book = (props) => {
  const { img, title, author, displayValue } = props;

  return (
    <article className='book'>
      <img src={img} alt={title} />
      <h2>{title}</h2>
      <button onClick={displayValue}>click me</button>
      <h4>{author} </h4>
    </article>
  );
};
```

#### More Complex Example

- initial setup
- create getBook function in booklist
- accepts id as an argument and finds the book
- [Javascript Nuggets - Filter and Find](https://www.youtube.com/watch?v=KeYxsev737s&list=PLnHJACx3NwAfRUcuKaYhZ6T5NRIpzgNGJ&index=4)
- pass the function down to Book Component and invoke on the button click
- in the Book Component destructure id and function
- invoke the function when user clicks the button, pass the id
- the goal : you should see the same book in the console

```js
const BookList = () => {
  const getBook = (id) => {
    const book = books.find((book) => book.id === id);
    console.log(book);
  };

  return (
    <section className='booklist'>
      {books.map((book) => {
        return <Book {...book} key={book.id} getBook={getBook} />;
      })}
    </section>
  );
};

const Book = (props) => {
  const { img, title, author, getBook, id } = props;
  // console.log(props);

  return (
    <article className='book'>
      <img src={img} alt={title} />
      <h2>{title}</h2>
      {/* this is not going to work */}
      <button onClick={getBook(id)}>display title</button>
      <h4>{author}</h4>
    </article>
  );
};
```

- two fixes
- first option - setup wrapper
```js
const Book = (props) => {
  const { img, title, author, getBook, id } = props;
  // console.log(props);
  const getSingleBook = () => {
    getBook(id);
  };
  return (
    <article className='book'>
      <img src={img} alt={title} />
      <h2>{title}</h2>
      <button onClick={getSingleBook}>display title</button>
      <h4>{author}</h4>
    </article>
  );
};
```

- two fixes
- second option - wrap in the anonymous arrow function

```js
const Book = (props) => {
  const { img, title, author, getBook, id } = props;
  // console.log(props);
  const getSingleBook = () => {
    getBook(id);
  };
  return (
    <article className='book'>
      <img src={img} alt={title} />
      <h2>{title}</h2>

      <button onClick={() => getBook(id)}>display title</button>
      <h4>{author}</h4>
    </article>
  );
};
```

#### Import and Export Statements

- remove all getBook code

```js
function BookList() {
  return (
    <section className='booklist'>
      {books.map((book) => {
        return <Book {...book} key={book.id} />;
      })}
    </section>
  );
}

const Book = (props) => {
  const { img, title, author } = props;

  return (
    <article className='book'>
      <img src={img} alt={title} />
      <h2>{title}</h2>

      <h4>{author} </h4>
    </article>
  );
};
```

- setup two files in src books.js and Book.js
- cut books array from index.js
- add to books.js

books.js

```js
const books = [
  {
    author: 'Jordan Moore',
    title: 'Interesting Facts For Curious Minds',
    img: './images/book-1.jpg',
    id: 1,
  },
  {
    author: 'James Clear',
    title: 'Atomic Habits',
    img: 'https://images-na.ssl-images-amazon.com/images/I/81wgcld4wxL._AC_UL900_SR900,600_.jpg',
    id: 2,
  },
];
```

- two flavors named and default exports

  - with named exports names MUST match
  - with default exports,can rename but only one per file

- named export

```js
export const books = [
  {
    author: 'Jordan Moore',
    title: 'Interesting Facts For Curious Minds',
    img: './images/book-1.jpg',
    id: 1,
  },
  {
    author: 'James Clear',
    title: 'Atomic Habits',
    img: 'https://images-na.ssl-images-amazon.com/images/I/81wgcld4wxL._AC_UL900_SR900,600_.jpg',
    id: 2,
  },
];
```

index.js

```js
import { books } from './books';
```

- default export

```js
const Book = (props) => {
  const { img, title, author } = props;

  return (
    <article className='book'>
      <img src={img} alt={title} />
      <h2>{title}</h2>

      <h4>{author} </h4>
    </article>
  );
};

export default Book;
```

index.js

```js
import Book from './Book';
```

#### Local Images (src folder)

- better performance because optimized
- add one more book to array
- download all three images (rename)
- setup images folder in the src
- import all three images in the books.js
- set image property equal to import
- and yes each image requires new import

```js
import img1 from './images/book-1.jpg';
import img2 from './images/book-2.jpg';
import img3 from './images/book-3.jpg';

export const books = [
  {
    author: 'Jordan Moore',
    title: 'Interesting Facts For Curious Minds',
    img: img1,
    id: 1,
  },
  {
    author: 'James Clear',
    title: 'Atomic Habits',
    img: img2,
    id: 2,
  },
  {
    author: 'Stephen King',
    title: 'Fairy Tale',
    img: img3,
    id: 3,
  },
];
```

#### Challenges

- setup numbers
- don't worry about css
- hint - index (second parameter in map)

index.js

```js
const BookList = () => {
  return (
    <section className='booklist'>
      {books.map((book, index) => {
        return <Book {...book} key={book.id} number={index} />;
      })}
    </section>
  );
};

const Book = (props) => {
  const { img, title, author, number } = props;

  return (
    <article className='book'>
      <img src={img} alt={title} />
      <h2>{title}</h2>

      <h4>{author}</h4>
      <span className='number'>{`# ${number + 1}`}</span>
    </article>
  );
};
```

index.css

```css
.book {
  background: #fff;
  border-radius: 1rem;
  padding: 2rem;
  text-align: center;
  /* set relative */
  position: relative;
}

.number {
  position: absolute;
  top: 0;
  left: 0;
  font-size: 1rem;
  padding: 0.75rem;
  border-top-left-radius: 1rem;
  border-bottom-right-radius: 1rem;
  background: #c35600;
  color: #fff;
}
```

#### Add Title

- add a title to our app (css optional)
- change page title

index.js

```js
function BookList() {
  return (
    <>
      <h1>amazon best sellers</h1>
      <section className='booklist'>
        {books.map((book) => {
          return <Book {...book} key={book.id} />;
        })}
      </section>
    </>
  );
}
```



#### Vite Doc(Vite)
#### Vite Setup

- need to use .jsx extension
- index.html in the source instead of public
- assets still in public
- instead of index.js, need to use main.jsx
- to spin up dev server - "npm run dev"

- rest the same - imports/exports, deployment, assets, etc...
