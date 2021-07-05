# reactNavbar
This React component consist of a responsive and automatically updating navbar.

Constructing this component helped me to better learn and practice the use of useRef().

A brief overview of the pertinent React code is given below.

First the Nabbar component is added into App.js to be rendered and connect the two files together.
```React
function App() {
  return (
    <>
      <Navbar />
    </>
  );
}
```

Next the return inside of Navbar.js is worked on.
```React
const Navbar = () => {
  return (
    <nav>
      <div className="nav-center">
        <div className="nav-header"></div>
        <div className="links-container show-container">
        <ul class='social-icons'></ul>
        </div>
      </div>
    </nav>
  );
};
```


Next work is started on the 'nav-header' <div>.
```React
        <div className="nav-header">
          <img src={logo} alt='logo'
          <button class='nav-toggle'>
          <FaBars />
          </button
        </div>
```


After this work is started on the 'links-container' <div>.
```React
        <div className="links-container">
          <ul className="links">
            <li>
              <a href='#'>Home</a>
            </li>
            <li>
              <a href='#'>about</a>
            </li>
            <li>
              <a href='#'>contact</a>
            </li>
            <li>
              <a href='#'>products</a>
            </li>
          </ul>
        </div>
```

Now work on the 'social-items' div.
```React
    <ul class='social-icons'>
      <li>
        <a href='https:www.twitter.com'>
          <FaTwitter />
        </a>
      </li>
        <a href='https:www.twitter.com'>
          <FaTwitter />
        </a>
      </li>
        <a href='https:www.twitter.com'>
          <FaTwitter />
        </a>
      </li>
    </ul>
```


The social media icons display on large screen only*
To avoid hard coding the links which would cause increased challenges with upkeep and updates a data.js file was setp up with the relevant data inserted inside an array of objects. This allows the data to be iterated over and monitored. To institute this method the links installed above will need to be deleted and the data iterated over, starting with the links.
```React
         <ul className="links">
            {links.map((link) => {
              const { id, url, text } = link;
              return (
                <li key={id}>
                  <a href={url}>{text}</a>
                </li>
              );
            })}
        </ul>
```


Now the links will be automatically updated across whenever edits to the data.js file are made. The same process now repeats for the social icons.
```React
        <ul className="social-icons">
          {social.map((socialIcon) => {
            const { id, url, icon } = socialIcon;
            return (
              <li key={id}>
                <a href={url}>{icon}</a>
              </li>
            );
          })}
        </ul>
```


Now that functionality has been set up it is time to deal with the toggle icon to show or hide the nav links upon a click event.
```React
const Navbar = () => {
  const [showLinks, setShowLinks] = useState(false);
  .......
          <button
            className="nav-toggle"
            onClick={() => {
              setShowLinks(!showLinks);
            }}
          >
            <FaBars />
          </button>
```


Now conditional rendering is used to render the div.
```React
    </div>
      {showLinks && (
      <div className="links-container" ref={linksContainerRef}>
          <ul className="links" ref={linksRef}>
            {links.map((link) => {
              const { id, url, text } = link;
              return (
                <li key={id}>
                  <a href={url}>{text}</a>
                </li>
              );
            })}
          </ul>
        </div>
```


Now the button hides and displays on click, however, this method is not suitable because it does not happen smoothly. This is because the current method is mounting the components together which is a process that does not allow for animations. Instead a different method will be used which utilizes the .links-container{} property inside of the style.css file. Only when the 'show-container' class is applied will the div render to the screen. To achieve this it is inserted dynamically.
```React
<div class={`${showLinks ? 'links-container show-container' : 'links-container'}`}>
```


Now there is a smooth animation upon the toggle button click. However, a final issue is that currently the height of the container is hard coded. So if any links are added or removed the container will not adjust at all. This will be fixed to auto-adjust the height depending on the number of links.
```React
<div class='links-container'>
  <ul class='links'>
    {links...}
```


Now useRef() is set up, one for the container and one for the links.
```React
const Navbar = () => {
  const [showLinks, setShowLinks] = useState(false);
  const linksContainerRef = useRef(null);
  const linksRef = useRef(null);
```


Now add the ref attribute
```React
<div class='links-container' ref={linksContainerRef}>
  <ul class='links' ref={linksRef}>
    {links.map((....
```


With these useRefs set up, everytime the value for showLink changes the useEffect will now be called.
```React
  useEffect(() => {
    const linksHeight = linksRef.current.getBoundingClientRect().height;
    if (showLinks) {
      linksContainerRef.current.style.height = `${linksHeight}px`;
    } else {
      linksContainerRef.current.style.height = "0px";
    }
  }, [showLinks]);
```

Now whenever the showLinks changes the height for the links is checked and depending on that value the height of the container will change automatically. To hide @ small screens remember that at the 800px media query the .links-container{} property must have height: auto !important set to override the inline CSS.
Lastly, render to the screen.
```React
ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```


***End walkthrough





