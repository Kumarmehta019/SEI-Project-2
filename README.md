
### README progress... ![90%](https://progress-bar.dev/90)

# SEI Project Two: Yu-Gi-Oh!

## Table of Contents:

|  **No:**     | **Content** |
| -------- | ------- |
|    1    | **`Project Overview`**|
|    2     | **`Project Brief and Technical Requirements`**|
|    3    | **`Technologies Used`**|
|    4     | **`Project Timeline - 2 Days`**|
|    5     | **`Bugs`**|
|    6     | **`Wins and Challenges`**|
|    7     | **`Future Improvements`**|
|    8     | **`Key Learnings`**|

 ## 1. Project Overview
This was a pair coding project, where we were briefed to **build a React application** that consumes a **public API**. My coding partner (Isaac) and I both had to decide on a common theme for the project. We both made a list of interests we had and from that we both had a common interest in Yu-Gi-Oh and decided to use the Yu-Gi-Oh API to build a website.

<img width="1866" alt="Screenshot 2021-12-14 at 16 55 42" src="https://user-images.githubusercontent.com/88886169/146043756-d5f8dc86-e23a-4421-ae6c-d7d6099bf3de.png">

#### Deployed version available here: 👉🏽👉🏽[*Yu-Gi-Oh!*](https://isaac-kumar-yugioh.netlify.app/) 👈🏽👈🏽

### Resources:
> The following websites assisted with this project:
> 1. [React](https://reactjs.org/)
> 2. [Some free APIs](https://apilist.fun/)
> 3. [Some more free APIs](https://github.com/public-apis/public-apis)
> 4. [Even more free APIs](https://dev.to/camerenisonfire/10-intriguing-public-rest-apis-for-your-next-project-2gbd)

## 2. Project Brief and Technical Requirements

- **Consume a public API**
- **A working application hosted on the browser**
- **Have several components**
- **The app can have a router**
- **Include wireframes**


## 3. Technologies Used

- React.js.
- JavaScript.
- HTML5.
- CSS.
- Bulma- CSS Framework.
- Animate.css.
- Axios.
- Git.
- GitHub.
- Google Fonts.
- Figma- Wireframe.
- Insomnia- REST Client.
- Yarn.


## 4. Project Timeline- 2 Days

### Planning:
The first day was spent finding a common interest between my coding partner and I. It was then a matter of reviewing and breaking down the dataset/type we would receive from the API. The data was tested in Insomnia to make sure the requests were working correctly. We looked into different endpoints to see which one we would need and how we could manipulate them to display on the frontend.

<img width="1514" alt="Screenshot 2021-12-14 at 17 17 45" src="https://user-images.githubusercontent.com/88886169/146047645-19e09a57-2927-4984-a311-ed6b8d13e365.png">

Once we were comfortable with the dataset it was then time to create a wireframe of the components and how they would all relate to each other. We wanted a homepage with a search functionality as well as three card categories (monster, spell and trap) displaying a variety of cards on different pages. Each card within the selected category would then be clickable so that the user is taken to a seperate page where they can read up on the details of the specific card they clicked.

<img width="976" alt="Screenshot 2021-12-14 at 17 32 45" src="https://user-images.githubusercontent.com/88886169/146049877-8d95f00d-38c6-4cd0-856d-d17cb437e0d3.png">

### Getting Started:
We started coding our project by creating the app using the command `npx create-react-app APP_NAME --template cra-template-ga-ldn`, adding the origin of our repo name to GitHub and pushing up. Then once inside the app we installed `yarn` and then typed in the command `yarn start` to run the server. We then installed `Bulma`, `React-Router-Dom`, `Axios` and `Animate.css` as these were the dependencies we needed.

### Routes:
The routes to the various pages/components were built using `React` as well as `BrowserRouter`, `Switch` and `Route` from `React-Router-Dom`.

```javascript
    <BrowserRouter>
      <NavBar />
      <Switch>
        <Route exact path='/' component={Home}/>
        <Route exact path='/spells' component={SpellIndex}/>
        <Route exact path='/traps' component={TrapIndex}/>
        <Route exact path='/monsters' component={MonsterIndex}/>
        <Route exact path='/wishlist' component={WishList}/>
        <Route exact path='/:id' component={CardShow}/>
        
      </Switch>
    </BrowserRouter>
```




### Components:

Once we had installed our dependencies for the project we started building out the different components we needed, as per our wireframe. 

**Navbar:** We used Bulma to assist us in obtaining a Navbar component which we felt was suitable for our website and then began adding the home, random and wishlist buttons. Once these buttons and the background of the Navbar component was designed, we began by chaining a get request to get the three different categories of cards. We were then able to create a nested onClick function that first randomised a card from one of the three categories and then sent the user to a specific page showing the id of the card. 

```javascript
const NavBar = () => {
  const history = useHistory()
  const [cards, setCards] = useState([])
  const [allCardId, setAllCardId] = useState([])
  const [hasError, setHasError] = useState(false)

  useEffect(() => {
    const getData = async () => {
      try {
        const { data } = await axios.get('https://db.ygoprodeck.com/api/v7/cardinfo.php?type=Normal%20Monster,spell%20card,trap%20card')
        // console.log('data', data)
        setCards(data.data)
      } catch (err) {
        console.log(err)
        setHasError(true)
      }
    }
    getData()
    
  }, [])

  function sendToRandom() {
    {allCardId ?
      history.push(`/${allCardId.id}`)
      : 
      hasError, 'Something went wrong 🆘'
    }
    
  }

  function handleRandom() {
    const item = [Math.floor(Math.random() * cards.length)]
    setAllCardId(cards[item])
    
    sendToRandom()
 
  }
```

**Homepage:** We bagan creating the homepage by adding a background using CSS, three buttons (one for each card category) and a search bar. We then built out the homepage by using `Link` import from `React-Router-Dom` to navigate to the three card category pages. For finishing touches, we added some animations to the buttons.

```javascript
const Home = () => {

  return (
    <section className='section' id='hero'>
      <div className='hero-body'>
        <br/>
        <br/>
        <br/>
        <br/>
        <br/>
        <br/>
        <div className='field'>
          <p className='control has-icons-right'>
            <input className='input is-medium is-link' type='search' placeholder='Search...' />
            <span className='icon is-large is-right'>
              <i className='search'>⏎</i>
            </span>
          </p>
        </div>
        <div className='has-text-centered' id='buttons'>
          <Link to='/spells'><button className='button is-success is-medium is-rounded has-text-weight-bold mx-2 has-text-black animate__animated animate__pulse animate__slower animate__infinite'>Spell Cards</button></Link>
          <Link to='/traps'><button className='button is-danger is-medium is-rounded has-text-weight-bold mx-2 has-text-black animate__animated animate__pulse animate__slower animate__infinite'>Trap Cards</button></Link>
          <Link to='/monsters'><button className='button is-warning is-medium is-rounded has-text-weight-bold mx-2 has-text-black animate__animated animate__pulse animate__slower animate__infinite'>Monster Cards</button></Link>
        </div>
      </div>
    </section>
  )

}
export default Home

```

**Monster, Spell & Trap pages:** These card category components were created one after another using different API endpoint get requests, they showed all the Yu-Gi-Oh cards based on the category. The data received was then displayed on the corresponding page using the `.map` method on the array of cards. An `IndexMap.js` helpers file was created to store props for how the cards should be displayed and was imported into the component to use for each card category page (rather than repeating the code over and over again).

```javascript
import React from 'react'
import { Link } from 'react-router-dom'


const IndexMap = (props) => {

  return (
    <div key={props.id} className='column is-one-fifth-desktop'>
      <Link to={`/${props.id}`}>
        <div className='card'>
          <div className='card-header has-background-black'>
            <div className='card-header-title is-centered cardTitle has-text-white is-underlined pl-0 pr-0 pt-3 pb-3 '>{props.name}</div>
          </div>
          <div className='card-image animate__animated animate__pulse animate__infinite animate__slower'>
            <figure className='image is-1'>
              <img src={props.card_images[0].image_url} alt={props.name} />
            </figure>
          </div>
          <div className='card-content pl-0 pr-0 pt-2 pb-1 has-text-centered has-background-black has-text-white has-text-weight-bold'>
            <h5 className="price">£ {props.card_prices[0].ebay_price}</h5>
          </div>
        </div>
      </Link>
    </div>
  )

}
export default IndexMap
```
<img width="1594" alt="Monster Cards" src="https://user-images.githubusercontent.com/88886169/146175796-062894b8-f491-44d8-a909-b6351510c267.png">
<img width="1594" alt="Spell Cards" src="https://user-images.githubusercontent.com/88886169/146175810-24040fc8-c1b2-41ff-aa4a-5c9a3bac7c8b.png">
<img width="1594" alt="Trap Cards" src="https://user-images.githubusercontent.com/88886169/146175815-51e594bd-731f-4b58-b785-b0c38a87b891.png">

**Card Show:** The card show component is a page that would display further information about each card (image, name, description and its price). Buttons to add and remove the card from the user's wishlist were also added. The url for each cocktail was used using the id of the card which we then added to `App.js` as an additional file path route `<Route path="/cocktail/:id">`. We used the id `useParams` from `react-router-dom` to add the id of the card to the API get request and then display this information on the page. We also attempted to add cards to the user's `Wish List` page by creating `add to Wish List`  and `remove from Wish List` buttons. We then created an onclick function on each button that would either add the id of the card or remove the id of the card from local storage. We, unfortunately, ran out of time to build out the add and remove to Wish List feature.

```javascript
import React, { useEffect, useState } from 'react'
import axios from 'axios'
import { useParams, Link } from 'react-router-dom'
import 'animate.css'

const CardShow = () => {

  const [chosenCard, setChosenCard] = useState(null)
  const [hasError, setHasError] = useState(false)

  const { id } = useParams()

  useEffect(() => {
    const getData = async () => {
      try {
        const { data } = await axios.get(`https://db.ygoprodeck.com/api/v7/cardinfo.php?id=${id}`)
        setChosenCard(data.data)
        console.log('Chosen card', chosenCard)
      } catch (err) {
        setHasError(true)
      }
    }
    getData()

  }, [id])

  const handleAdd = () => {
    window.localStorage.setItem(`wishListCard${window.localStorage.length}`, id)
  }

  const handleRemove = () => {
    for (let i = 0; i < window.localStorage.length; i++) {
      const myValue = window.localStorage.getItem(`wishListCard${i}`)
      if (myValue === id) {
        window.localStorage.removeItem(`wishListCard${i}`)
      }
    }
  }

```


<img width="1581" alt="Card Show" src="https://user-images.githubusercontent.com/88886169/146180806-753afb21-94ff-4fc4-be26-64a72f76a25c.png">

**Wish List:** The Wish List component is a page that displays the user's cards that they have added to the wishlist. Unfortunately, we had run out of time to implement this feature. We wanted the user to have a collection of cards that they had added to the wishlist and there would be a total cost of buying these cards.

**Styling:**

The layout was created using the Bulma framework and with a bit of help from CSS. This really helped to provide the site with continuity and structure across all the pages of the website. Animate.css was used to animate the website and give various functions on the website an aesthetically pleasing feel.


## 5. Bugs

- The random button doesnt work when you first click on it but when you click on it again, it works.
- The Wish List on the card show page doesn't work properly, but this may also be due to running out of time.

## 6. Wins and Challenges

### Wins:

- Pair Coding - this was my first time pair coding on a project. Isaac and I worked really well together and really played to each other's strengths and weaknesses. Having an extra pair of eyes on a project really helps to overcome errors that each of us might have. It also improves your learning on topics/areas you require further development on.

- Functionality- Given the short time we were given, I am amazed at how far we came and what we were able to build. If given more time I wonder how much more we could have built out this website and completed the components we didn't get around to completing.

- React and APIs- Using React and a public API to create a website which displays the information to a user in a fun and aesthetically pleasing way.

### Challenges:

- Time - one of the biggest challenges we faced during this project was completing the project to a standard we were both happy with. We were unable to implement the search and the Wish List functionality, which we could have if we were given more time. These however were stretch goals for us as our main purpose was to display the three card categories and individual card show page. 

- Bulma CSS Framework - Figuring out how to use the different components of Bulma and what variations you could make to these took some time to figure out, but it was also a win as we both knew how to use Bulma by the end of the project.

## 7. Future Improvements

- Fix the bugs.
- Make the website mobile responsive.
- complete the Wish List functionality.
- complete the search functionality.


## 8. Key Learnings

- Learning to use React `useEffect` and `useState` as well as the React-Router-Dom. 
- Learning to use Bulma CSS Framework.
- Utilising Insomnia to test API endpoints and get requests.
- Creating a wireframe on Figma.
- Learning to work as a pair and working around each other's schedule.


