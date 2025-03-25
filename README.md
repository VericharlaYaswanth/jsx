import { useState, useEffect } from "react";
import "./App.css";

const slideshowImages = [
  "beach-wedding.png",
  "private-party.png",
  "seminars.png",
  "product-launch.png",
  "workshops.png",
];

const services = [
  { name: "Weddings", image: "wedding.png" },
  { name: "Corporate Events", image: "corporate.png" },
  { name: "Private Parties", image: "party.png" },
  { name: "Catering Services", image: "catering.png" },
  { name: "Photography", image: "photography.png" },
  { name: "Music & Entertainment", image: "music.png" },
];

const weddingOptions = [
  { name: "Beach Wedding", image: "beach-wedding.png" },
  { name: "Hotel Wedding", image: "hotel-wedding.png" },
  { name: "Church Wedding", image: "chruch-wedding.png" },
  { name: "Destination Wedding", image: "destination-wedding.png" },
];

const decorationOptions = [
  { name: "Regular", price: 50000, image: "Regular.png" },
  { name: "Melody", price: 125000, image: "Melody.png" },
  { name: "Royal", price: 250000, image: "Royal.png" },
  { name: "Elegant", price: 300000, image: "Elegant.png" },
];

const photographerOptions = [
  { name: "Melody", price: 25000, image: "photographer-melody.png" },
  { name: "Elegant", price: 40000, image: "photographer-elegant.png" },
  { name: "Mayuka Special", price: 75000, image: "photographer-mayuka.png" },
];

const clients = [
  "Rahul Sharma",
  "Ananya Iyer",
  "Vikram Singh",
  "Priya Desai",
  "Rajesh Kumar",
];

export default function App() {
  const [loading, setLoading] = useState(true);
  const [loggedIn, setLoggedIn] = useState(false);
  const [userName, setUserName] = useState("");
  const [currentImage, setCurrentImage] = useState(0);
  const [page, setPage] = useState("home");
  const [selectedWedding, setSelectedWedding] = useState(null);
  const [selectedDecoration, setSelectedDecoration] = useState(false);
  const [selectedPackage, setSelectedPackage] = useState(null);
  const [seatingCapacity, setSeatingCapacity] = useState(0);
  const [cartItems, setCartItems] = useState([]);
  const [menuOpen, setMenuOpen] = useState(false);
  const [showCart, setShowCart] = useState(false);
  const [eventDate, setEventDate] = useState("");
  const [eventTime, setEventTime] = useState("");
  const [eventAmPm, setEventAmPm] = useState("AM");
  const [eventPlace, setEventPlace] = useState("");
  const [eventDistrict, setEventDistrict] = useState("");
  const [eventState, setEventState] = useState("");
  const [eventCountry, setEventCountry] = useState("");
  const [showEventDetails, setShowEventDetails] = useState(false);
  const [selectedPhotographer, setSelectedPhotographer] = useState(null);
  const [priestRequired, setPriestRequired] = useState(false);

  // New state for Fully Managed options
  const [fullyManaged, setFullyManaged] = useState(false);
  const [numberOfGuests, setNumberOfGuests] = useState(0);
  const [cateringOption, setCateringOption] = useState("");
  const [entertainmentOption, setEntertainmentOption] = useState("");

  useEffect(() => {
    setTimeout(() => setLoading(false), 2000);
    const interval = setInterval(() => {
      setCurrentImage((prev) => (prev + 1) % slideshowImages.length);
    }, 2000);
    return () => clearInterval(interval);
  }, []);

  const handleLogin = () => {
    setLoggedIn(true);
    setUserName("John Doe");
  };

  const handleAddToCart = () => {
    if (seatingCapacity <= 0) {
      alert("Please enter a valid seating capacity!");
      return;
    }

    if (!eventDate || !eventTime || !eventPlace || !eventDistrict || !eventState || !eventCountry) {
      alert("Please fill in all event details!");
      return;
    }

    // Calculate total price including photographer and priest if selected
    let totalPrice = seatingCapacity * 30 + selectedPackage.price;
    if (selectedPhotographer) {
      totalPrice += selectedPhotographer.price;
    }
    if (priestRequired) {
      totalPrice += 15000; // Adding priest fee
    }

    const cartItem = {
      weddingType: selectedWedding,
      decorationPackage: selectedPackage.name,
      seatingCapacity,
      totalPrice,
      date: eventDate,
      time: `${eventTime} ${eventAmPm}`,
      location: {
        place: eventPlace,
        district: eventDistrict,
        state: eventState,
        country: eventCountry,
      },
      photographer: selectedPhotographer ? selectedPhotographer.name : null,
      priestRequired,
      fullyManaged: fullyManaged ? {
        numberOfGuests,
        cateringOption,
        entertainmentOption,
      } : null,
    };
    
    setCartItems([...cartItems, cartItem]);
    alert("Item added to cart!");
    
    // Reset all states
    setSelectedWedding(null);
    setSelectedDecoration(false);
    setSelectedPackage(null);
    setSeatingCapacity(0);
    setEventDate("");
    setEventTime("");
    setEventAmPm("AM");
    setEventPlace("");
    setEventDistrict("");
    setEventState("");
    setEventCountry("");
    setShowEventDetails(false);
    setFullyManaged(false);
    setNumberOfGuests(0);
    setCateringOption("");
    setEntertainmentOption("");
    setSelectedPhotographer(null);
    setPriestRequired(false);
    setPage("home");
  };

  const handleRemoveFromCart = (index) => {
    const newCartItems = [...cartItems];
    newCartItems.splice(index, 1);
    setCartItems(newCartItems);
  };

  const handleContinueToEventDetails = () => {
    setShowEventDetails(true);
  };

  if (loading) {
    return (
      <div className="loading-screen">
        <img src="/logo.png" alt="Logo" className="logo" />
        <div className="loading-bar-container">
          <div className="loading-bar"></div>
        </div>
      </div>
    );
  }

  if (!loggedIn) {
    return (
      <div className="login-container">
        <h1 className="title">Welcome to Mayuka</h1>
        <p className="subtitle">Login to manage your events</p>
        <input className="input-box" placeholder="Email" />
        <input className="input-box" type="password" placeholder="Password" />
        <button className="login-btn" onClick={handleLogin}>Login</button>
        <p className="forgot-password">Forgot Password?</p>
      </div>
    );
  }

  return (
    <div className="home-container">
      <nav className="navbar">
        <div className="nav-left">
          <div className="menu-container">
            <button className="menu-btn" onClick={() => setMenuOpen(!menuOpen)}>⋮</button>
            {menuOpen && (
              <div className="menu-dropdown">
                <button onClick={() => setPage("history")}>History</button>
              </div>
            )}
          </div>
          <img src="/logo.png" alt="Logo" className="nav-logo" />
        </div>
        <div className="nav-right">
          {userName && <span className="user-info">Welcome, {userName}</span>}
          <button className="nav-btn" onClick={() => setPage("home")}>Home</button>
          <button className="nav-btn">About Us</button>
          <button className="nav-btn">Services</button>
          <button className="cart-btn" onClick={() => setShowCart(!showCart)}>
            🛒 Cart ({cartItems.length})
          </button>
        </div>
      </nav>

      {showCart && (
        <div className="cart-container">
          <h2>Your Cart</h2>
          {cartItems.length === 0 ? (
            <p>Your cart is empty</p>
          ) : (
            <div className="cart-items">
              {cartItems.map((item, index) => (
                <div key={index} className="cart-item">
                  <div className="cart-item-details">
                    <h3>{item.weddingType}</h3>
                    <p>Decoration: {item.decorationPackage}</p>
                    <p>Seating Capacity: {item.seatingCapacity}</p>
                    <p>Photographer: {item.photographer || "None"}</p>
                    <p>Priest Required: {item.priestRequired ? "Yes" : "No"}</p>
                    <p>Date: {item.date}</p>
                    <p>Time: {item.time}</p>
                    <p>Location: {item.location.place}, {item.location.district}, {item.location.state}, {item.location.country}</p>
                    {item.fullyManaged && (
                      <>
                        <p>Number of Guests: {item.fullyManaged.numberOfGuests}</p>
                        <p>Catering Option: {item.fullyManaged.cateringOption}</p>
                        <p>Entertainment Option: {item.fullyManaged.entertainmentOption}</p>
                      </>
                    )}
                    <p>Total Price: Rs {item.totalPrice.toLocaleString()}</p>
                  </div>
                  <button className="remove-btn" onClick={() => handleRemoveFromCart(index)}>Remove</button>
                </div>
              ))}
              <div className="cart-total">
                <h3>Total: Rs {cartItems.reduce((total, item) => total + item.totalPrice, 0).toLocaleString()}</h3>
                <button className="checkout-btn">Proceed to Checkout</button>
              </div>
            </div>
          )}
          <button className="close-cart-btn" onClick={() => setShowCart(false)}>Close</button>
        </div>
      )}

      {page === "history" && <h2>Event History</h2>}

      {page === "home" && (
        <>
          <div className="slideshow-container">
            <img src={slideshowImages[currentImage]} alt="Slideshow" className="slideshow-img" />
            <h1 className="welcome-text">Welcome to Mayuka</h1>
          </div>

          <section className="services-section">
            <h2>Our Services</h2>
            <div className="service-grid">
              {services.map((service, index) => (
                <button key={index} className="service-card" onClick={() => setPage(service.name)}>
                  <img src={service.image} alt={service.name} className="service-img" />
                  <h3 className="service-name">{service.name}</h3>
                </button>
              ))}
            </div>
          </section>

          <section className="clients-section">
            <h2>Client Testimonials</h2>
            <p>"Mayuka made our wedding an unforgettable experience!"</p>
            <p>"Outstanding service and perfect event execution!"</p>
          </section>
        </>
      )}

      {page === "Weddings" && (
        <div className="wedding-options">
          <h2>Select Wedding Type</h2>
          <div className="wedding-grid">
            {weddingOptions.map((option, index) => (
              <button key={index} className="wedding-option-card" onClick={() => setSelectedWedding(option.name)}>
                <img src={option.image} alt={option.name} className="wedding-option-img" />
                <h3 className="wedding-option-name">{option.name}</h3>
              </button>
            ))}
          </div>
        </div>
      )}

      {selectedWedding && (
        <div className="wedding-management-options">
          <h2>Select Management Option for {selectedWedding}</h2>
          <div className="management-grid">
            <button className="management-option" onClick={() => setSelectedDecoration(true)}>
              Only Decoration
            </button>
            <button className="management-option" onClick={() => setFullyManaged(true)}>
              Fully Managed
            </button>
          </div>
        </div>
      )}

      {selectedDecoration && (
        <div className="decoration-options">
          <h2>Select Decoration Package</h2>
          <div className="decoration-grid">
            {decorationOptions.map((option, index) => (
              <div key={index} className="decoration-card" onClick={() => setSelectedPackage(option)}>
                <img src={option.image} alt={option.name} className="decoration-img" />
                <h3 className="decoration-name">{option.name}</h3>
                <p className="decoration-price">Rs {option.price.toLocaleString()}</p>
              </div>
            ))}
          </div>
        </div>
      )}

      {selectedPackage && !showEventDetails && (
        <div className="additional-options-section">
          <div className="seating-capacity-section">
            <h2>Enter Seating Capacity</h2>
            <input
              type="number"
              placeholder="Seating Capacity"
              value={seatingCapacity}
              onChange={(e) => setSeatingCapacity(Number(e.target.value))}
              className="seating-input"
            />
          </div>

          <div className="photographer-section">
            <h2>Select Photographer Package</h2>
            <div className="photographer-grid">
              {photographerOptions.map((option, index) => (
                <div 
                  key={index} 
                  className={`photographer-card ${selectedPhotographer?.name === option.name ? 'selected' : ''}`}
                  onClick={() => setSelectedPhotographer(option)}
                >
                  <img src={option.image} alt={option.name} className="photographer-img" />
                  <h3 className="photographer-name">{option.name}</h3>
                  <p className="photographer-price">Rs {option.price.toLocaleString()}</p>
                </div>
              ))}
              <div 
                className={`photographer-card ${selectedPhotographer === null ? 'selected' : ''}`}
                onClick={() => setSelectedPhotographer(null)}
              >
                <h3 className="photographer-name">No Photographer</h3>
                <p className="photographer-price">Rs 0</p>
              </div>
            </div>
          </div>

          <div className="priest-section">
            <h2>Priest Required?</h2>
            <div className="priest-options">
              <button 
                className={`priest-option ${priestRequired ? 'selected' : ''}`}
                onClick={() => setPriestRequired(true)}
              >
                Yes (Rs 15,000)
              </button>
              <button 
                className={`priest-option ${!priestRequired ? 'selected' : ''}`}
                onClick={() => setPriestRequired(false)}
              >
                No
              </button>
            </div>
          </div>

          <div className="price-summary">
            <h3>Price Breakdown</h3>
            <p>Base Price: Rs {selectedPackage.price.toLocaleString()}</p>
            <p>Seating Cost: Rs {(seatingCapacity * 30).toLocaleString()} (Rs 30 per seat)</p>
            {selectedPhotographer && (
              <p>Photographer: Rs {selectedPhotographer.price.toLocaleString()}</p>
            )}
            {priestRequired && (
              <p>Priest: Rs 15,000</p>
            )}
            <h3>Total Price: Rs {(
              seatingCapacity * 30 + 
              selectedPackage.price + 
              (selectedPhotographer ? selectedPhotographer.price : 0) + 
              (priestRequired ? 15000 : 0)
            ).toLocaleString()}</h3>
          </div>

          <button className="add-to-cart-btn" onClick={handleContinueToEventDetails}>
            Continue to Event Details
          </button>
        </div>
      )}

      {fullyManaged && selectedWedding === "Beach Wedding" && (
        <div className="fully-managed-options">
          <h2>Fully Managed Beach Wedding</h2>
          <div className="form-group">
            <label>Number of Guests:</label>
            <input
              type="number"
              placeholder="Number of Guests"
              value={numberOfGuests}
              onChange={(e) => setNumberOfGuests(Number(e.target.value))}
              className="event-input"
            />
          </div>
          <div className="form-group">
            <label>Catering Option:</label>
            <select
              value={cateringOption}
              onChange={(e) => setCateringOption(e.target.value)}
              className="event-input"
            >
              <option value="">Select Catering Option</option>
              <option value="Vegetarian">Vegetarian</option>
              <option value="Non-Vegetarian">Non-Vegetarian</option>
              <option value="Both">Both</option>
            </select>
          </div>
          <div className="form-group">
            <label>Entertainment Option:</label>
            <select
              value={entertainmentOption}
              onChange={(e) => setEntertainmentOption(e.target.value)}
              className="event-input"
            >
              <option value="">Select Entertainment Option</option>
              <option value="DJ">DJ</option>
              <option value="Live Band">Live Band</option>
              <option value="Classical Music">Classical Music</option>
            </select>
          </div>

          <div className="photographer-section">
            <h2>Select Photographer Package</h2>
            <div className="photographer-grid">
              {photographerOptions.map((option, index) => (
                <div 
                  key={index} 
                  className={`photographer-card ${selectedPhotographer?.name === option.name ? 'selected' : ''}`}
                  onClick={() => setSelectedPhotographer(option)}
                >
                  <img src={option.image} alt={option.name} className="photographer-img" />
                  <h3 className="photographer-name">{option.name}</h3>
                  <p className="photographer-price">Rs {option.price.toLocaleString()}</p>
                </div>
              ))}
              <div 
                className={`photographer-card ${selectedPhotographer === null ? 'selected' : ''}`}
                onClick={() => setSelectedPhotographer(null)}
              >
                <h3 className="photographer-name">No Photographer</h3>
                <p className="photographer-price">Rs 0</p>
              </div>
            </div>
          </div>

          <div className="priest-section">
            <h2>Priest Required?</h2>
            <div className="priest-options">
              <button 
                className={`priest-option ${priestRequired ? 'selected' : ''}`}
                onClick={() => setPriestRequired(true)}
              >
                Yes (Rs 15,000)
              </button>
              <button 
                className={`priest-option ${!priestRequired ? 'selected' : ''}`}
                onClick={() => setPriestRequired(false)}
              >
                No
              </button>
            </div>
          </div>

          <button className="add-to-cart-btn" onClick={handleContinueToEventDetails}>
            Continue to Event Details
          </button>
        </div>
      )}

      {showEventDetails && (
        <div className="event-details-section">
          <h2>Event Details</h2>
          <div className="event-details-form">
            <div className="form-group">
              <label>Date:</label>
              <input
                type="date"
                value={eventDate}
                onChange={(e) => setEventDate(e.target.value)}
                className="event-input"
              />
            </div>

            <div className="form-group time-group">
              <label>Time:</label>
              <div className="time-inputs">
                <input
                  type="text"
                  placeholder="HH:MM"
                  value={eventTime}
                  onChange={(e) => setEventTime(e.target.value)}
                  className="time-input"
                />
                <select
                  value={eventAmPm}
                  onChange={(e) => setEventAmPm(e.target.value)}
                  className="ampm-select"
                >
                  <option value="AM">AM</option>
                  <option value="PM">PM</option>
                </select>
              </div>
            </div>

            <div className="form-group">
              <label>Place:</label>
              <input
                type="text"
                placeholder="Venue Name"
                value={eventPlace}
                onChange={(e) => setEventPlace(e.target.value)}
                className="event-input"
              />
            </div>

            <div className="form-group">
              <label>District:</label>
              <input
                type="text"
                placeholder="District"
                value={eventDistrict}
                onChange={(e) => setEventDistrict(e.target.value)}
                className="event-input"
              />
            </div>

            <div className="form-group">
              <label>State:</label>
              <input
                type="text"
                placeholder="State"
                value={eventState}
                onChange={(e) => setEventState(e.target.value)}
                className="event-input"
              />
            </div>

            <div className="form-group">
              <label>Country:</label>
              <input
                type="text"
                placeholder="Country"
                value={eventCountry}
                onChange={(e) => setEventCountry(e.target.value)}
                className="event-input"
              />
            </div>

            <div className="event-summary">
              <h3>Event Summary</h3>
              <p>Wedding Type: {selectedWedding}</p>
              <p>Decoration Package: {selectedPackage.name}</p>
              <p>Seating Capacity: {seatingCapacity}</p>
              <p>Photographer: {selectedPhotographer ? selectedPhotographer.name : "None"}</p>
              <p>Priest Required: {priestRequired ? "Yes" : "No"}</p>
              {fullyManaged && (
                <>
                  <p>Number of Guests: {numberOfGuests}</p>
                  <p>Catering Option: {cateringOption}</p>
                  <p>Entertainment Option: {entertainmentOption}</p>
                </>
              )}
              <p>Total Price: Rs {(
                seatingCapacity * 30 + 
                selectedPackage.price + 
                (selectedPhotographer ? selectedPhotographer.price : 0) + 
                (priestRequired ? 15000 : 0)
              ).toLocaleString()}</p>
            </div>

            <button className="add-to-cart-btn" onClick={handleAddToCart}>
              Add to Cart
            </button>
          </div>
        </div>
      )}

      <footer className="footer">
        <p>&copy; 2025 Mayuka Event Management. All rights reserved.</p>
      </footer>
    </div>
  );
}
