# ❖	React Shopping Cart
## ⮚	App.js
``` javascript
import BootstrapCart from "./bootstrap-cart/BootstrapCart.js";
import SimpleCart from "./simple-cart/SimpleCart.js";
import "./App.css";
import { Link } from "react-router-dom";
import { useState } from "react";
import { Button, Form } from "react-bootstrap";

function App() {
  const [bootstrapCart, SetBootstrapCart] = useState(true);
  return (
    <div className="App">
      <div className="cart-navbar">
          <span href="/" >React Shopping Cart</span>
        
        <div className="cart-toggle">
          <Form>
            <Form.Check
              type="switch"
              id="custom-switch"
              label="Click to Switch into Different Cart"
              onClick={() =>
                bootstrapCart ? SetBootstrapCart(false) : SetBootstrapCart(true)
              }
            />
          </Form>
        </div>
      </div>
      {bootstrapCart ? (
        <div class="grid-container">
          <div class="grid-item-active">CART-1</div>
          <div class="grid-item-deactive">CART 2</div>
        </div>
      ) : (
        <div class="grid-container">
          <div class="grid-item-deactive">CART 1</div>
          <div class="grid-item-active">CART 2</div>
        </div>
      )}

      {bootstrapCart ? <BootstrapCart /> : <SimpleCart />}
    </div>
  );
}

export default App;
```

## ⮚	App.css
``` css
.grid-container {
  display: grid;
  grid-template-columns: auto auto;
  font-size: 15px;
  text-align: center;
  padding: 1% 10%;
  height: 60px;
}

.grid-item-active {
  background-color: black;
  padding: 10px;
  color: white;
}
.grid-item-deactive {
  background-color: white;
  color: black;
  padding: 10px;
}
.cart-navbar {
  display: grid;
  grid-template-columns: auto auto;
  padding: 5px;
  background-color: #212529;
  color: white;
}
.cart-navbar > span {
  font-size: 25px;
  font-weight: 400;
  padding-left: 30px;
}
.cart-toggle {
  margin-top: 8px;
}

@media screen and (max-width: 600px) {
    .cart-navbar > span {
        font-size: 20px;
        font-weight: 300;
      }
  }
```


## ⮚	bootsrap-cart/components/CartCard.js

``` javascript
import React from "react";
import { AiFillDelete } from "react-icons/ai";
import { CartState } from "../context/Context.js";
const CartCard = (props) => {
  const {
    dispatch,
  } = CartState();
  // console.log("CartCard", cart);

  return (
    <div className="cart-product-container" key={props.product.id}>
      <img className="cart-product-image" src={props.product.image} alt="img" />
      <div className="cart-item-details">
        <div className="cart-product-title">{props.product.name}</div>
        <div className="cart-product-price">
          Rs {props.product.price}
        </div>
      </div>

      <div className="cart-delete-icon">
        <AiFillDelete
          fontSize="20px"
          style={{ cursor: "pointer" }}
          onClick={() =>
            dispatch({ type: "REMOVE_FROM_CART", payload: props.product })
          }
        />
      </div>
    </div>
  );
};

export default CartCard;
```

## ⮚	bootsrap-cart/components/CheckOutPage.js

``` javascript
import React, { useState, useEffect } from "react";
import { CartState } from "../context/Context.js";
import { AiFillDelete } from "react-icons/ai";
import {
  ListGroup,
  Button,
  Row,
  Col,
  Form,
  Image,
} from "react-bootstrap";
import Rating from "./Rating.js";
const CheckOutPage = () => {
  const {
    state: { cart },
    dispatch,
  } = CartState();

  
  // let initial = 0;
  // cart.map((p) => {
  //   initial = initial + p.price.split(".")[0] * p.qty;
  //   console.log(p.price.split(".")[0], p.qty, p.price.split(".")[0] * p.qty);
  //   console.log(initial);
  //   setTotal(initial);
  // });

  // function FunctionTotal(total, number){
  //   return total + number;
  // }
  const [total, setTotal] = useState(0);
  useEffect(() => {
    setTotal(
      // cart.reduce((FunctionTotal))
      cart.reduce((acc, curr) => acc + Number(curr.price*curr.qty), 0)
    );
  }, [cart]);
  // console.log(cart);
  // console.log(total);

  return (
    <>
      <div className="home">
        <div className="checkout-product-container">
          <ListGroup>
            {cart.map((product) => (
              <ListGroup.Item key={product.id}>
                <Row>
                  <Col md={2}>
                    <Image
                      src={product.image}
                      alt={product.name}
                      fluid
                      rounded
                    ></Image>
                  </Col>
                  <Col md={2}>
                    <span>{product.name}</span>
                  </Col>
                  <Col md={2}>
                    <span>{product.price}</span>
                  </Col>
                  <Col md={2}>
                    <Rating rating={product.ratings} />
                  </Col>
                  <Col md={2}>
                    <Form.Control
                      as="select"
                      value={product.qty}
                      onChange={(e) =>
                        dispatch({
                          type: "CHANGE_CART_QTY",
                          payload: { id: product.id, qty: e.target.value },
                        })
                      }
                    >
                      {[...Array(product.inStock).keys()].map((x) => (
                        <option key={x + 1}>{x + 1}</option>
                      ))}
                    </Form.Control>
                  </Col>
                  <Col>
                    <div className="cart-delete-icon">
                      <AiFillDelete
                        fontSize="20px"
                        style={{ cursor: "pointer" }}
                        onClick={() =>
                          dispatch({
                            type: "REMOVE_FROM_CART",
                            payload: product,
                          })
                        }
                      />
                    </div>
                  </Col>
                </Row>
              </ListGroup.Item>
            ))}
            
          </ListGroup>
        </div>
        <div className="checkout-summary">
          <span className="checkout-title">Subtotal ({cart.length}) items</span>
          <span className="checkout-title">Total: Rs {total}</span>
          <Button size="sm" style={{  }} disabled={cart.length === 0}>
            Proceed to Checkout
          </Button>
        </div>
      </div>
    </>
  );
};

export default CheckOutPage;
```

## ⮚	bootsrap-cart/components/Filter.js

``` javascript
// import React, { useState } from "react";
import { Form, Button, FormControl } from "react-bootstrap";
import Rating from "./Rating.js";
import { CartState } from "../context/Context.js";

export const Filter = () => {
  // const [rate, setRating] = useState(0);
  const {
    FilterState: { byRating, sort },
    FilterDispatch,
  } = CartState();

  return (
    <div className="filters">
        <span className="title">Filter Products</span>
        <span>
          <Form  >
            <FormControl
              size="sm"
              type="search"
              placeholder="Search a product"
              className="me-2"
              aria-label="Search"
              onChange={(e) =>
                FilterDispatch({
                  type: "FILTER_BY_SEARCH",
                  payload: e.target.value,
                })
              }
            />
          </Form>
        </span>
        <span>
          <Form.Check
            inline
            label="Price low to high"
            name="group1"
            type="radio"
            id={`inline-1`}
            onChange={() =>
              FilterDispatch({
                type: "SORT_BY_PRICE",
                payload: "LowToHigh",
              })
            }
            checked={sort === "lowToHigh" ? true : false}
          />
        </span>
        <span>
          <Form.Check
            inline
            label="Price high to low"
            name="group1"
            type="radio"
            id={`inline-2`}
            onChange={() =>
              FilterDispatch({
                type: "SORT_BY_PRICE",
                payload: "HighToLow",
              })
            }
            checked={sort === "HighToLow" ? true : false}
          />
        </span>
        <span>
          <Form.Check
            inline
            label="Remove Out Stock"
            name="group1"
            type="checkbox"
            id={`inline-3`}
            onChange={() =>
              FilterDispatch({
                type: "FILTER_BY_STOCK",
              })
            }
          />
        </span>
        <span>
          <Form.Check
            inline
            label="Fast Delivery"
            name="group1"
            type="checkbox"
            id={`inline-4`}
            onChange={() =>
              FilterDispatch({
                type: "FILTER_BY_DELIVERY",
              })
            }
          />
        </span>
        <span>
          <label>Rating</label>
          {/* <Rating
            rating={rate}
            onClick={(i) => setRating(i + 1)}
            style={{ cursor: "pointer" }}
          /> */}
          <Rating
            rating={byRating}
            onClick={(i) =>
              FilterDispatch({
                type: "FILTER_BY_RATING",
                payload: i + 1,
              })
            }
            style={{ cursor: "pointer" }}
          />
        </span>
        <Button
          size="sm"
          variant="success"
          onClick={() =>
            FilterDispatch({
              type: "CLEAR_FILTER",
            })
          }
        >
          Clear Filters
        </Button>
      </div>
  );
};
```

## ⮚	bootsrap-cart/components/Footer.js

``` javascript
import React from "react";

const Footer = () => (
  <>
    <div className="footer-container">
      © 2020 Manish :<a href="/"> made with love</a>
    </div>
  </>
);

export default Footer;
```

## ⮚	bootsrap-cart/components/Header.js

``` javascript
// import React, { useState } from "react";
import { AiFillHome, AiFillHeart } from "react-icons/ai";
import { Button, Dropdown } from "react-bootstrap";
import { CartState } from "../context/Context.js";
import { AiOutlineShoppingCart } from "react-icons/ai";
import { Link } from "react-router-dom";
import CartCard from "./CartCard.js";
import "../BootstrapCart.css";

const Header = () => {
  const {
    state: { cart },
  } = CartState();
  return (
    <div>
      <>
        <div className="navbar-container">
          <Link to="/">
            <AiFillHome
              style={{ color: "black", fontSize: "1.8rem", margin: 10 }}
            />
          </Link>
          <Link to="/WishListPage">
            <AiFillHeart
              style={{ color: "red", fontSize: "1.8rem", margin: 10 }}
            />
          </Link>
          <Dropdown className="drop-down">
            <Dropdown.Toggle variant="success" id="dropdown-basic">
              <AiOutlineShoppingCart
                style={{
                  color: "white",
                  fontSize: "1.3rem",
                  marginRight: 10,
                }}
              />
              {cart.length}
            </Dropdown.Toggle>
            <div className="cart-dropdown-fix">
              <Dropdown.Menu>
                <span>
                  {cart.length === 0 ? (
                    <div style={{ padding: 10 }}>Cart is Empty</div>
                  ) : (
                    <div>
                      {cart.map((product) => (
                        <CartCard product={product} key={product.id} />
                      ))}
                      <div className="button-box">
                        <Link to="/CheckOut" style={{ align: "center" }}>
                          <Button variant="success" size="sm">
                            Check Out
                          </Button>
                        </Link>
                      </div>
                    </div>
                  )}
                </span>
              </Dropdown.Menu>
            </div>
          </Dropdown>
        </div>
      </>
    </div>
  );
};

export default Header;
```

## ⮚	bootsrap-cart/components/ProductCard.js

``` javascript
import React from "react";
import { AiFillHeart, AiOutlineHeart } from "react-icons/ai";
import { Card, Button, ListGroup } from "react-bootstrap";
import Rating from "./Rating.js";
import { CartState } from "../context/Context.js";
const ProductCard = (props) => {
  // console.log(props);
  // console.log(props.product.image);
  // function randomDeliveryPeriod() {
  //   return Math.floor(Math.random() * 3 + 2);
  // }

  const {
    state: { cart, wishlist },
    dispatch,
  } = CartState();

  // console.log(cart);

  return (
    <div>
      <Card style={{ width: "18rem", marginBottom: 20 }} key={props.id}>
        <Card.Img variant="top" src={props.product.image} />
        <Card.Body>
          <Card.Title>{props.product.name}</Card.Title>
          {/* <p>{props.product.description}</p> */}
          <Card.Text>{props.product.description}</Card.Text>
          <ListGroup variant="flush">
            <Rating rating={props.product.ratings} />
            <span>Rs {props.product.price}</span>
            <span>
              {props.product.fastDelivery ? (
                <div>Fast Delivery</div>
              ) : (
                <div>{props.product.lateDelivery} Days</div>
              )}
            </span>
          </ListGroup>

          <div className="button-box">
            {/* WishLIst */}
            {wishlist.some((p) => p.id === props.product.id) ? (
              <AiFillHeart
                onClick={() => {
                  dispatch({
                    type: "REMOVE_FROM_WISHLIST",
                    payload: props.product,
                  });
                }}
                style={{ color: "red", fontSize: "1.8rem" }}
              />
            ) : (
              <AiOutlineHeart
                onClick={() => {
                  dispatch({
                    type: "ADD_TO_WISHLIST",
                    payload: props.product,
                  });
                }}
                style={{ color: "black", fontSize: "1.8rem" }}
              />
            )}

            {cart.some((p) => p.id === props.product.id) ? (
                 <Button size="sm">Added</Button>
                 
            ) : (
              <Button
                onClick={() => {
                  dispatch({
                    type: "ADD_TO_CART",
                    payload: props.product,
                  });
                }}
                variant="primary"
                disabled={!props.product.inStock}
                size="sm"
              >
                {props.product.inStock ? (
                  <div>Add to Cart</div>
                ) : (
                  <div>Out of Stock</div>
                )}
              </Button>
            )}
          </div>
        </Card.Body>
      </Card>
    </div>
  );
};

export default ProductCard;
```
## ⮚	bootsrap-cart/components/ProdutPage.js
``` javascript
import React from "react";
import ProductCard from "./ProductCard.js";
import { CartState } from "../context/Context";
import "../BootstrapCart.css";
import { Filter } from "./Filter.js";
const Products = () => {
  const {
    state: { products },
    FilterState: { byStock, byFastDelivery, byRating, sort, searchQuery },
  } = CartState();
  // console.log("PRODUCTS : ", products);
  // console.log("FILTER STATE : ", "byStock", byStock,"byFastDelivery", byFastDelivery,"byRating", byRating, "sort", sort,"searchQuery", searchQuery);

  const FilteredProduct = () => {
    let sortedProducts = products;
    if (sort) {
      sortedProducts = sortedProducts.sort((a, b) =>
        sort === "LowToHigh" ? a.price - b.price : b.price - a.price
      );
      // console.log("sort", sortedProducts);
    }
    if (byStock) {
      sortedProducts = sortedProducts.filter(
        (product) => product.inStock >= true
      );
      // console.log("byStock", sortedProducts);
    }

    if (byFastDelivery) {
      sortedProducts = sortedProducts.filter(
        (product) => product.fastDelivery === true
      );
      // console.log("byFastDelivery", sortedProducts);
    }
    if (searchQuery) {
      sortedProducts = sortedProducts.filter((product) =>
        product.name.toLowerCase().includes(searchQuery.toLowerCase())
      );
      console.log("searchQuery", sortedProducts);
    }
    if (byRating) {
      sortedProducts = sortedProducts.filter(
        (product) => product.ratings === byRating
      );
      // console.log("byRating", sortedProducts);
    }
    // console.log("sortedProducts", sortedProducts);
    return sortedProducts;
  };
  // const {
  //   FilterState,
  //   FilterDispatch
  // } = CartState();

  return (
    <>
      <div className="home">
        <div className="filterContainer">
          <Filter />
        </div>
        <div className="productContainer">
          {FilteredProduct().map((product) => (
            <ProductCard product={product} key={product.id} />
          ))}
        </div>
      </div>
    </>
  );
};

export default Products;
```

## ⮚	bootsrap-cart/components/Rating.js
``` javascript
import React from "react";
import { AiOutlineStar, AiFillStar } from "react-icons/ai";

const Rating = ({ rating, onClick, style }) => {
  return (
    <div>
      {[...Array(5)].map((_, i) => (
        <span key={i} onClick={() => onClick(i)} style={style}>
          {rating > i ? <AiFillStar /> : <AiOutlineStar />}
        </span>
      ))}
    </div>
  );
};

export default Rating;
```

## ⮚	bootsrap-cart/components/WishListPage.js
``` javascript
import React from "react";
import { CartState } from "../context/Context.js";
import { AiFillHeart, AiOutlineHeart } from "react-icons/ai";
import { ListGroup, Row, Col, Image } from "react-bootstrap";
import "../BootstrapCart.css";

import Rating from "./Rating.js";
import {Link} from "react-router-dom";

const WishListPage = () => {
  const {
    state: { wishlist },
    dispatch,
  } = CartState();
  console.log(wishlist.length);
  return (
    <div>
      {wishlist.length >=  1 ? <div><ListGroup>
        {wishlist.map((product) => (
          <ListGroup.Item>
            <Row>
              <Col md={2}>
                <Image
                  src={product.image}
                  alt={product.name}
                  fluid
                  rounded
                ></Image>
              </Col>
              <Col md={2}>
                <span>{product.name}</span>
              </Col>
              <Col md={2}>
                <span>{product.price}</span>
              </Col>
              <Col md={2}>
                <Rating rating={product.ratings} />
              </Col>
              <Col md={2}>
                {wishlist.some((p) => p.id === product.id) ? (
                  <AiFillHeart
                    onClick={() => {
                      dispatch({
                        type: "REMOVE_FROM_WISHLIST",
                        payload: product,
                      });
                    }}
                    style={{ color: "red", fontSize: "1.8rem" }}
                  />
                ) : (
                  <AiOutlineHeart
                    onClick={() => {
                      dispatch({
                        type: "ADD_TO_WISHLIST",
                        payload: product,
                      });
                    }}
                    style={{ color: "black", fontSize: "1.8rem" }}
                  />
                )}
              </Col>
            </Row>
          </ListGroup.Item>
        ))}
      </ListGroup></div> : <Link to="/"><div className="wishlist-button-text">Wishlist is Empty, Add some Products!</div></Link>
      }

      
    </div>
  );
};

export default WishListPage;
```

## ⮚	bootsrap-cart/context/Context.js
``` javascript
import { createContext, useReducer, useContext } from "react";
// import { faker } from "@faker-js/faker";
import { CartReducer, FilterReducer } from "./Reducer.js";
import data from "../product-data/data.json";

const Cart = createContext();

const Context = ({ children }) => {
  // const products = [...Array(20)].map(() => ({
  //   id: faker.datatype.uuid(),
  //   name: faker.commerce.productName(),
  //   price: faker.commerce.price(),
  //   image: faker.image.image(),
  //   inStock: faker.helpers.arrayElement([0, 3, 4, 8]),
  //   department: faker.commerce.department(),
  //   description: faker.commerce.productDescription(),
  //   fastDelivery: faker.datatype.boolean(),
  //   ratings: faker.helpers.arrayElement([1, 2, 3, 4, 5]),
  //   lateDelivery: Math.floor(Math.random() * 3 + 2),
  // }));

  const products = data;
  // console.log(products);

  const [state, dispatch] = useReducer(CartReducer, {
    products: products,
    cart: [],
    wishlist: [],
  });

  const [FilterState, FilterDispatch] = useReducer(FilterReducer, {
    byStock: false,
    byFastDelivery: false,
    sort: false,
    byRating: "",
    searchQuery: "",
  
  });
    // console.log("state : ", state);
    // console.log("dispatch : ", dispatch);
  return <Cart.Provider value={{ state, dispatch, FilterState, FilterDispatch }}>{children}</Cart.Provider>;
};

export default Context;

export const CartState = () => {
  return useContext(Cart);
};
```

## ⮚	bootsrap-cart/context/Reducer.js
``` javascript
export const CartReducer = (state, action) => {
  switch (action.type) {
    case "ADD_TO_CART":
      return { ...state, cart: [...state.cart, { ...action.payload, qty: 1 }] };
    case "REMOVE_FROM_CART":
      return {
        ...state,
        cart: state.cart.filter((c) => c.id !== action.payload.id),
      };
    case "ADD_TO_CART_ONLY":
      return {
        ...state,
        cart: state.cart.filter((c) =>
          c.id === action.payload.id ? (c.qty = c.qty + action.payload.qty) : c.qty
        ),
      };

    case "ADD_TO_WISHLIST":
      return {
        ...state,
        wishlist: [...state.wishlist, { ...action.payload, qty: 1 }],
      };

    case "REMOVE_FROM_WISHLIST":
      return {
        ...state,
        wishlist: state.wishlist.filter((e) => e.id !== action.payload.id),
      };

    case "CHANGE_CART_QTY":
      return {
        ...state,
        cart: state.cart.filter((c) =>
          c.id === action.payload.id ? (c.qty = action.payload.qty) : c.qty
        ),
      };
    default:
      return state;
  }
};

export const FilterReducer = (state, action) => {
  switch (action.type) {
    case "SORT_BY_PRICE":
      return {
        ...state,
        sort: action.payload,
      };
    case "FILTER_BY_STOCK":
      return {
        ...state,
        byStock: !state.byStock,
      };
    case "FILTER_BY_DELIVERY":
      return {
        ...state,
        byFastDelivery: !state.byFastDelivery,
      };
    case "FILTER_BY_RATING":
      return {
        ...state,
        byRating: action.payload,
      };
    case "FILTER_BY_SEARCH":
      return {
        ...state,
        searchQuery: action.payload,
      };
    case "CLEAR_FILTER":
      return {
        byStock: false,
        byFastDelivery: false,
        byRating: 0,
        searchQuery: "",
      };
    default:
      return state;
  }
};
```

## ⮚	bootsrap-cart/product-data/data.js
``` javascript
[
    {
        "id": "1",
        "name": "HERE&NOW",
        "image": "https://i.ibb.co/55cwBpP/2.png",
        "price": 300,
        "inStock": 2,
        "description": "Smooth comfortable product",
        "fastDelivery": true,
        "ratings": 4,
        "lateDelivery": 2        

    },
    {
        "id": "2",
        "name": "Calvin Klein Jeans",
        "image": "https://i.ibb.co/NNd5wmc/1.png",
        "price": 250,
        "inStock": 0,
        "description": "Cotton, foldable",
        "fastDelivery": false,
        "ratings": 5,
        "lateDelivery": 5
    },
    {
        "id": "3",
        "name": "Moda Rapido",
        "image": "https://i.ibb.co/wQzRVJM/3.png",
        "price": 304,
        "inStock": 1,
        "description": "No one can find this type",
        "fastDelivery": true,
        "ratings": 2,
        "lateDelivery": 1
    },
    {
        "id": "4",
        "name": "Roadster",
        "image": "https://i.ibb.co/Fh2Bv0D/4.png",
        "price": 400,
        "inStock": 0,
        "description": "You can't afford it",
        "fastDelivery": true,
        "ratings": 5,
        "lateDelivery": 2
    },
    {
        "id": "5",
        "name": "Difference of Opinion",
        "image": "https://i.ibb.co/Vq5KDbx/5.png",
        "price": 300,
        "inStock": 2,
        "description": "Wear or not, it's your choice",
        "fastDelivery": false,
        "ratings": 1,
        "lateDelivery": 10
    },
    {
        "id": "6",
        "name": "H&M",
        "image": "https://i.ibb.co/X867M8m/6.png",
        "price": 306,
        "inStock": 5,
        "description": "Smooth comfortable product",
        "fastDelivery": true,
        "ratings": 4,
        "lateDelivery": 3
    },
    {
        "id": "7",
        "name": "Roadster",
        "image": "https://i.ibb.co/YcdG2pK/7.png",
        "price": 390,
        "inStock": 1,
        "description": "Silky",
        "fastDelivery": false,
        "ratings": 4,
        "lateDelivery": 13
    },
    {
        "id": "8",
        "name": "FIFO DIDO",
        "image": "https://i.ibb.co/FbC6z7C/8.png",
        "price": 450,
        "inStock": 3,
        "description": "real, authentic product",
        "fastDelivery": false,
        "ratings": 3,
        "lateDelivery": 3
    },
    {
        "id": "9",
        "name": "Roadster",
        "image": "https://i.ibb.co/xg6vqVd/9.png",
        "price": 320,
        "inStock": 2,
        "description": "No mercy",
        "fastDelivery": false,
        "ratings": 3,
        "lateDelivery": 7
    },
    {
        "id": "10",
        "name": "HERE&NOW",
        "image": "https://i.ibb.co/wzzghxL/10.png",
        "price": 300.90,
        "inStock": 2,
        "description": "It's your turn to buy",
        "fastDelivery": true,
        "ratings": 5,
        "lateDelivery": 2
    },
    {
        "id": "11",
        "name": "jack & jones",
        "image": "https://i.ibb.co/jZ11DSd/11.png",
        "price": 100,
        "inStock": 1,
        "description": "Light weight",
        "fastDelivery": true,
        "ratings": 4,
        "lateDelivery": 2
    },
    {
        "id": "12",
        "name": "Roadster",
        "image": "https://i.ibb.co/SK6pX6B/12.png",
        "price": 200,
        "inStock": 1,
        "description": "New in stock",
        "fastDelivery": false,
        "ratings": 1,
        "lateDelivery": 12
    },
    {
        "id": "13",
        "name": "Roadster",
        "image": "https://i.ibb.co/gDKrGxY/13.png",
        "price": 450,
        "inStock": 5,
        "description": "You friends will definitely like it",
        "fastDelivery": false,
        "ratings": 3,
        "lateDelivery": 10
    },
    {
        "id": "14",
        "name": "Calvin Klein Jeans",
        "image": "https://i.ibb.co/w6QGNv9/14.png",
        "price": 500,
        "inStock": 1,
        "description": "Manish's choice",
        "fastDelivery": false,
        "ratings": 1,
        "lateDelivery": 8
    },
    {
        "id": "15",
        "name": "Maniac",
        "image": "https://i.ibb.co/5BtcNN8/15.png",
        "price": 203,
        "inStock": 3,
        "description": "buy it without hustle",
        "fastDelivery": true,
        "ratings": 3,
        "lateDelivery": 3
    },
    {
        "id": "16",
        "name": "HERE&NOW",
        "image": "https://i.ibb.co/xDcvk2V/16.png",
        "price": 300.90,
        "inStock": 0,
        "description": "Last and effortless",
        "fastDelivery": true,
        "ratings": 4,
        "lateDelivery": 1
    }
]
```

## ⮚	bootsrap-cart/BootstrapCart.js
``` javascript
import React from "react";
import Header from "./components/Header.js";
import Footer from "./components/Footer.js";
import CheckOutPage from "./components/CheckOutPage.js";
import Products from "./components/ProductPage.js";
import { BrowserRouter, Routes, Route } from "react-router-dom";
import WishListPage from "./components/WishListPage.js";
import "./BootstrapCart.css";

function main() {
  return (
    <>
      <BrowserRouter>
        <Routes>
          <Route
            exact
            path="/"
            element={
              <>
                <Header />
                <Products />
                <Footer />
              </>
            }
          />
          <Route
            exact
            path="/WishListPage"
            element={
              <>
                <Header />
                <WishListPage />
                <Footer />
              </>
            }
          />
          <Route
            exact
            path="/CheckOut"
            element={
              <>
                <Header />
                <CheckOutPage />
                <Footer />
              </>
            }
          />
        </Routes>
      </BrowserRouter>
    </>
  );
}

export default main;
```

## ⮚	bootsrap-cart/BootstrapCart.css
``` css
* {
  margin: 0px;
}
a {
  style: none;
}
.home {
  display: flex;
  flex-direction: row;
}

.filterContainer {
  display: flex;
  width: 23%;
  float: left;
  padding: 10px;
  flex-wrap: wrap;
  justify-content: space-around;
}
.productContainer {
  display: flex;
  width: 75%;
  float: right;
  padding: 10px;
  flex-wrap: wrap;
  justify-content: space-around;
}

.filters {
  height: 532px;
  color: white;
  background-color: #212529;
  border-radius: 4px;
  padding: 20px;
  display: flex;
  flex-direction: column;
}
.title {
  font-size: 23px;
  font-weight: 700;
}
.filters > span {
  margin-bottom: 5px;
}

.button-box {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  margin-top: 20px;
}

.cart-product-container {
  padding: 10px;
  display: flex;
  justify-content: space-around;
  align-items: center;
  margin: 0 20px;
  margin-bottom: 20px;
  flex-direction: row;
  width: 200px;
  left: 0px;
}
.cart-product-image {
  width: 50px;
  height: 50px;
  object-fit: cover;
  border-radius: 50%;
}
.drop-down {
  margin: 10px;
}
.cart-item-details {
  display: flex;
  flex: 1;
  padding: 0 20px;
  flex-direction: column;
}
.cart-product-title {
  font-size: 14px;
  font-weight: 600;
}
.cart-product-price {
  font-size: 12px;
  font-weight: 300;
}
.cart-delete-icon {
  right: 0px;
}
.checkout-product-container {
  display: flex;
  border: 2px solid;
  width: 60%;
  min-height: 300px;
  float: left;
  margin-top: 20px;
  margin-left: 20px;
  padding: 10px;
}

.checkout-summary {
  right: 0px;
  width: 30%;
  overflow-wrap: right;
  float: right;
  padding: 20px;
  position: absolute;
  margin-right: 20px;
  margin-top: 20px;
  background-color: #212529;
  color: white;
  display: flex;
  flex-direction: column;
}
.checkout-title {
  font-size: 1.5rem;
  font-weight: 600;
}
.button {
}
@media screen and (max-width: 700px) {

  .checkout-title {
    font-size: 1rem;
    font-weight: 200;
  }
  .checkout-summary {
    text-align: center;
    display: flex;
    margin: 20px;
    flex-wrap: wrap;
    width: 80%;
    justify-content: space-around;
    position: inherit;
  }
  .checkout-product-container {
    text-align: center;
    display: flex;
    padding: 20px;
    flex-wrap: wrap;
    width: 80%;
    justify-content: space-around;
  }

  .home {
    display: flex;
    flex-direction: column;
  }
  .productContainer {
    display: flex;
    padding: 20px;
    flex-wrap: wrap;
    width: 100%;
    justify-content: space-around;
  }
  .filterContainer {
    display: flex;
    padding: 20px;
    flex-wrap: wrap;
    width: 100%;
    justify-content: space-around;
  }
  
  .filters {
    color: white;
    background-color: #212529;
    border-radius: 4px;
    padding: 20px;
    height: 100%;
  }

  .filters > span {
    margin-bottom: 5px;
  }

}

.footer-container {
  color: white;
  background-color: #212529;
  bottom: 0px;
  margin-top: 200px;
  /* margin: 32px; */
  align-items: center;
  display: flex;
  text-align: center;
  justify-content: center;
  padding: 20px;
  /* position: fixed; */
  width: -webkit-fill-available;
}
.wishlist-button-text {
  color: black;
  margin-top: 100px;
  margin-bottom: 100px;
  align-items: center;
  display: flex;
  text-align: center;
  justify-content: center;
  padding: 20px;
  width: -webkit-fill-available;
}
.navbar-container {
  display: flex;
  flex-direction: row;
} ```

## ⮚	simple-cart/SimpleCart.js
``` javascript
import React, { useState, useEffect } from "react";
import Sizes from "./components/Sizes";
import Products from "./components/Products";
import Cart from "./components/Cart";
import filterList from "./components/filterList";
import "./SimpleCart.css";
import { AiOutlineShoppingCart } from "react-icons/ai";

import {
  Dropdown,
} from "react-bootstrap";

const SimpleCart = () => {
  const [products, setProducts] = useState([]);
  const [selectedSizes, setSelectedSizes] = useState([]);
  const [cart, setCart] = useState([]);

  console.log(
    "products",
    products,
    "selectedSizes",
    selectedSizes,
    "cart",
    cart
  );

  useEffect(() => {
    setProducts(filterList([], null));
    if (JSON.parse(localStorage.getItem("cart"))) {
      setCart(JSON.parse(localStorage.getItem("cart")));
    }
  }, []);

  const setSize = (size) => {
    const sizes = [...selectedSizes];

    if (sizes.includes(size)) {
      const index = sizes.indexOf(size);
      sizes.splice(index, 1);
    } else {
      sizes.push(size);
    }
    setSelectedSizes(sizes);
    setProducts(filterList(sizes, "size"));
  };

  const sortProducts = (method) => {
    const array = products;

    if (method === "Lowest to Highest") {
      array.sort(function (a, b) {
        return a.price - b.price;
      });
    } else if (method === "Highest to Lowest") {
      array.sort(function (a, b) {
        return b.price - a.price;
      });
    }
    setProducts(array);
  };

  const addToCart = (item) => {
    const productList = [...cart];
    if (!productList.includes(item)) {
      productList.push(item);
    }
    const index = productList.indexOf(item);
    productList[index].quantity++;
    setCart(productList);
    localStorage.setItem("cart", JSON.stringify(productList));
  };

  const changeQuantity = (item, e) => {
    const productList = [...cart];
    const index = productList.indexOf(item);
    if (e === "+") {
      productList[index].quantity++;
    } else {
      if (productList[index].quantity > 1) {
        productList[index].quantity--;
      } else {
        productList.splice(index, 1);
      }
    }
    setCart(productList);
    localStorage.setItem("cart", JSON.stringify(productList));
  };
  return (
    <div>
      <Dropdown className="drop-down">
        <Dropdown.Toggle variant="success" id="dropdown-basic">
          <AiOutlineShoppingCart
            style={{
              color: "white",
              fontSize: "1.3rem",
              marginRight: 10,
            }}
          />
        </Dropdown.Toggle>
        <Dropdown.Menu>
        <Cart products={cart} changeQuantity={changeQuantity} />
        </Dropdown.Menu>
      </Dropdown>

      <div className="home-container">
        <Sizes selectedSizes={selectedSizes} setSize={setSize} />
        <Products
          products={products}
          sortProducts={sortProducts}
          addToCart={addToCart}
        />
        {/* <Cart products={cart} changeQuantity={changeQuantity} /> */}
      </div>
    </div>
  );
};

export default SimpleCart;
```

## ⮚	simple-cart/ SimpleCart.css
``` css
.sizes {
    margin: 5px;
  }
  
  .size-list {
    display: flex;
    flex-wrap: wrap;
  }
  
  .size {
    width: 38px;
    height: 38px;
    padding: 0;
    border: 0;
    font-size: 11px;
    border-radius: 50%;
    margin-right: 14px;
    margin-bottom: 12px;
    cursor: pointer;
    font-weight: bold;
  } 
  
  .selected-size {
    background: #1b1920;
    color: white;
  }
  
  .size:hover {
    border: 1px solid black;
  }
  
  .size:focus {
    outline: none;
  }
  
  .loader {
    width: 200px;
    display: block;
    margin: auto;
  }
  
  .products {
    margin: 5px;
  }
  
  .products-nav {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    margin-block-start: 1em;
    margin-block-end: 1em;
  }
  
  .products-nav h3 {
    margin: 0;
  }
  
  .sort-list select {
    font-size: 1rem;
    padding: 4px 6px;
    background: inherit;
    border: 1px solid #cccccc;
    border-radius: 4px;
  }
  
  .sort-list select:focus {
    outline-color: #196ead;
  }
  
  .products-length {
    font-size: 0.9rem;
    color: hsl(0, 2%, 47%)
  }
  
  .card-list {
    display: flex;
    flex-wrap: wrap;
  }
  
  .card {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 10px auto;
    border: 1px solid #cccccc;
    border-radius: 4px;
    padding: 14px 10px;
  }
  
  .card-title {
    margin-top: 9px;
    margin-bottom: 3px;
    color: #10212e;;
    cursor: pointer;
  
  }
  
  .price {
    margin-top: 0;
    margin-bottom: 8px;
  }
  
  .card-image {
    width: 200px;
    height: auto;
  }
  

  .cart {
    position: fixed;
    top: 0;
    right: 0;
  }
  
  /* cart */
  
 
  .toggle-btn {
    position: absolute;
    right: 520px;
    cursor: pointer;
  }
  
  .toggle-btn span {
    display: block;
    width: 35px;
    height: 5px;
    background: #151719;
    margin: 5px 0px;
  }
  
  .cart-content{
    width: 300px;
    height: 100%;
    background: rgb(21, 23, 25);
    /* right: -500px; */
    transition: all 300ms linear 0s;
  }
  .cart-content h3 {
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    color: white;
    font-size: 2rem;
    border-bottom: 2px solid blue;
  }
  
  .cart-content h3 img {
    width: 50px;
    margin: 10px;
  }
    
 
  
  .subtotal-div {
    display: flex;
    justify-content: space-between;
    padding: 20px;
  }
  
  .subtotal {
    color: white;
    font-weight: bolder;
    font-size: 1.4rem;
  }
  
  .subtotal-price {
    color: white;
  }
  
  .checkout-btn {
    margin-top: 20px;
    width: 100%;
    padding: 12px;
    font-weight: bold;
    color: white;
    background: #196ead;
    font-size: 1.2rem;
    border: 0;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.7s;
  }
  
  .checkout-btn:hover {
    background: midnightblue;
  }
  
  .empty-cart {
    color: white;
    padding: 25px;
    text-align: center;
    justify-content: center;
    font-size: 1.4rem;
    line-height: 1.5;
    letter-spacing: 0.07ch;
  }
  
  .cart-item {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    padding: 14px;
    border-bottom: 2px solid rgb(100, 100, 100, 0.3);
    color: #fff;
  }
  
  .cart-item > div {
    flex-grow: 1;
    margin-left: 10px;
  }
  
  .cart-item > div > div {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  .cart-item p {
    margin: 6px 0;
  }
  
  .cart-item-image {
    width: 74px;
  }
  
  .item-title {
    color: darkgoldenrod;
    font-weight: bolder;
    font-size: 1.25rem;
    text-transform: uppercase;
  }
  
  .item-quantity {
    color: cadetblue;
  }
  
  .cart-item-price {
    color: #196ead;
    font-size: 1.4rem;
  }
  
  .quantity-btn {
    margin-left: 7px;
    font-size: 1.1rem;
    border: 0;
    padding: 0;
    width: 20px;
    height: 20px;
    color: white;
    background: #535353;
    cursor: pointer;
  }
  .home-container{
    width: 76%;
    display: grid;
    grid-template-columns: 16% auto;
    margin: 5vh auto;
  }
  
  @media only screen and (max-width: 1000px) {
    .App {
      grid-template-columns: 100%;
    }
  
    .sizes {
      margin: 0;
      text-align: center;
    }
  
    .sizes h3 {
      margin: 10px auto;
    }
  
    .size-list {
      justify-content: center;
    }
  
    .size {
      margin-right: 6.5px;
    }
  
    .sort-list {
      margin: auto;
    }
  
    .products-nav h3 {
      margin: 10px auto;
    }
  
    .products-length {
      display: flex;
      justify-content: center;
    }
  
    #sidebar {
      width: 100%;
      height: 100%;
      background: #151719;
      right: -100%;
      transition: all 300ms linear;
    }
  
    .toggle-btn {
      position: fixed;
      top: 20px;
      right: 20px;
      cursor: pointer;
    }
  
    #sidebar.active .toggle-btn {
      z-index: 1;
    }
  
    #sidebar.active .toggle-btn span {
      background: white;
      transition: transform 0.5s;
    }
  
    #sidebar.active .toggle-btn .span-1 {
      transform: rotate(45deg);
    }
  
    #sidebar.active .toggle-btn .span-2 {
      display: none !important;
    }
  
    #sidebar.active .toggle-btn .span-3 {
      transform: rotate(135deg);
      position: relative;
      top: -10px;
    }
  
    .cart-list {
      height: 54vh;
    }
  }
  ```

## ⮚	simple-cart/components/Cart.js
``` javascript
import React, { Fragment, useState, useEffect } from "react";

const Cart = ({ products, changeQuantity }) => {
  const [sum, setSum] = useState(0);
  useEffect(() => {
    let total = 0;
    for (var i = 0; i < products.length; i++) {
      total += products[i].price * products[i].quantity;
    }
    setSum(total);
  }, [products]);

  const checkout = () => {
    alert(`Checkout - Subtotal: $ ${sum.toFixed(2)}`);
  };

  return (
    <Fragment>
      <div className="cart-content">
        <h3>
          <img
            src="https://checkout.advancedshippingmanager.com/wp-content/uploads/2015/10/Cart-Icon-PNG-Graphic-Cave-e1461785088730-300x300.png"
            alt="cart"
          />
          Cart
        </h3>

        {products.length === 0 ? (
          <div className="empty-cart">
            There are no items in Cart, Please add items to checkout!!!
          </div>
        ) : (
          products.map((product) => {
            return (
              <>
                <div className="cart-item">
                  <img
                    src={product.url}
                    alt="cart-item"
                    className="cart-item-image"
                  />
                  <div>
                    <div>
                      <p className="item-title">{product.title}</p>
                      <span className="cart-item-price">
                        Rs {product.price}
                      </span>
                    </div>
                    <div>
                      <p className="item-quantity">
                        Quantity: <span>{product.quantity}</span>
                      </p>
                      <div>
                        <button
                          className="quantity-btn"
                          onClick={() => changeQuantity(product, "-")}
                        >
                          -
                        </button>
                        <button
                          className="quantity-btn"
                          onClick={() => changeQuantity(product, "+")}
                        >
                          +
                        </button>
                      </div>
                    </div>
                  </div>
                </div>
              </>
            );
          })
        )}
        <div>
          <div className="subtotal-div">
            <p className="subtotal">SUBTOTAL</p>
            <p className="subtotal-price">Rs {sum.toFixed(2)}</p>
          </div>
        </div>

        <button className="checkout-btn" onClick={checkout}>
          CHECKOUT
        </button>
      </div>
    </Fragment>
  );
};

export default Cart;
```


## ⮚	simple-cart/components/filterList.js
``` javascript
import products from '../data/products.json'

export default function filterList(arr, method) {

    if(method == null) return products;

    else {
          return products.filter(product => {
          const sizeArray = product.size.split(" ");
          if(arr.length > 0) {
                if(sizeArray.some(r => arr.indexOf(r) >= 0)) {
                    return product;
            }
          }
          else {
              return products;
          }
      })  
    }
}
```

## ⮚	simple-cart/components/Product.js
``` javascript
import React, { useState } from "react";
import { Button } from "react-bootstrap";

const Products = ({ products, sortProducts, addToCart }) => {
  const [value, setValue] = useState("Select");
//   const [clickable, setClickable] = useState(true);
//   console.log(clickable);
 
  const setList = (e) => {
    setValue(e.target.value);
    sortProducts(e.target.value);
  };

  return (
    <div className="products">
      <div className="products-nav">
        <h3>Products</h3>
        <div className="sort-list">
          Sort by&nbsp;: &nbsp;
          <select value={value} onChange={setList}>
            <option value="Select">Select</option>
            <option value="Highest to Lowest">Highest to Lowest</option>
            <option value="Lowest to Highest">Lowest to Highest</option>
          </select>
        </div>
      </div>

      <div className="card-list">
        {products.length === 0 ? (
          <p className="text-center">No Products Available!</p>
        ) : (
          products.map((product) => {
            return (
              <div className="card" key={product.id}>
                <img
                  src={product.url}
                  className="card-image"
                  alt="product"
                  title={product.title}
                />
                <h3 className="card-title">{product.title}</h3>
                <p className="price">
                  price: <span>Rs {product.price}</span>
                </p>
                <p className="price">
                  size: <span> {product.size}</span>
                </p>
                {/* {clickable ? (
                  <Button
                    size="sm"
                    variant="secondary"
                    onClick={() =>
                      addToCart(product) && setClickable(!clickable)
                    }
                  >
                    Add to cart
                  </Button>
                ) : (
                  <Button size="sm" variant="secondary">
                    Added
                  </Button>
                )} */}
                  <Button
                    size="sm"
                    variant="secondary"
                    onClick={() =>
                      addToCart(product)
                    }
                  >
                    Add to cart
                  </Button>
              </div>
            );
          })
        )}
      </div>
    </div>
  );
};

export default Products;
 ```

## ⮚	simple-cart/components/Sizes.js
``` javascript
import React from 'react'

const Sizes = ({selectedSizes, setSize}) =>  {

    const sizes = ['XS', 'S', 'M', 'ML', 'L', 'XL', 'XXL'];

    return (
        <div className="sizes">
            <h3>Sizes</h3>
            <div className="size-list">
                {
                    sizes.map((size, index) => {
                        return (
                            <button 
                                className={ "size" + (selectedSizes.includes(size) ? " selected-size" : "")}
                                key={index}
                                onClick={() => setSize(size)}
                            >
                                {size}
                            </button>
                        )
                    })
                }
            </div>
        </div>
    )
}

export default Sizes;
```

## ⮚	simple-cart/data/products.json
``` javascript
[
    {
        "id": "1",
        "title": "HERE&NOW",
        "url": "https://i.ibb.co/55cwBpP/2.png",
        "size": "M",
        "price": 300.90,
        "quantity": 0
    },
    {
        "id": "2",
        "title": "Calvin Klein Jeans",
        "url": "https://i.ibb.co/NNd5wmc/1.png",
        "size": "XL",
        "price": 200.00,
        "quantity": 0
    },
    {
        "id": "3",
        "title": "Moda Rapido",
        "url": "https://i.ibb.co/wQzRVJM/3.png",
        "size": "S",
        "price": 300.80,
        "quantity": 0
    },
    {
        "id": "4",
        "title": "Roadster",
        "url": "https://i.ibb.co/Fh2Bv0D/4.png",
        "size": "L",
        "price": 200.99,
        "quantity": 0
    },
    {
        "id": "5",
        "title": "Difference of Opinion",
        "url": "https://i.ibb.co/Vq5KDbx/5.png",
        "size": "M",
        "price": 210.90,
        "quantity": 0
    },
    {
        "id": "6",
        "title": "H&M",
        "url": "https://i.ibb.co/X867M8m/6.png",
        "size": "S",
        "price": 800.50,
        "quantity": 0
    },
    {
        "id": "7",
        "title": "Roadster",
        "url": "https://i.ibb.co/YcdG2pK/7.png",
        "size": "XXL",
        "price": 220.99,
        "quantity": 0
    },
    {
        "id": "8",
        "title": "FIFO DIDO",
        "url": "https://i.ibb.co/FbC6z7C/8.png",
        "size": "M",
        "price": 200.00,
        "quantity": 0
    },
    {
        "id": "9",
        "title": "Roadster",
        "url": "https://i.ibb.co/xg6vqVd/9.png",
        "size": "ML",
        "price": 240.80,
        "quantity": 0
    },
    {
        "id": "10",
        "title": "HERE&NOW",
        "url": "https://i.ibb.co/wzzghxL/10.png",
        "size": "ML",
        "price": 270.99,
        "quantity": 0
    },
    {
        "id": "11",
        "title": "jack & jones",
        "url": "https://i.ibb.co/jZ11DSd/11.png",
        "size": "ML L",
        "price": 290.99,
        "quantity": 0
    },
    {
        "id": "12",
        "title": "Roadster",
        "url": "https://i.ibb.co/SK6pX6B/12.png",
        "size": "L",
        "price": 215.90,
        "quantity": 0
    },
    {
        "id": "13",
        "title": "Roadster",
        "url": "https://i.ibb.co/gDKrGxY/13.png",
        "size": "XXL",
        "price": 216.00,
        "quantity": 0
    },
    {
        "id": "14",
        "title": "Calvin Klein Jeans",
        "url": "https://i.ibb.co/w6QGNv9/14.png",
        "size": "L",
        "price": 214.60,
        "quantity": 0
    },
    {
        "id": "15",
        "title": "Maniac",
        "url": "https://i.ibb.co/5BtcNN8/15.png",
        "size": "XL",
        "price": 210.50,
        "quantity": 0
    },
    {
        "id": "16",
        "title": "HERE&NOW",
        "url": "https://i.ibb.co/xDcvk2V/16.png",
        "size": "XXL",
        "price": 212.00,
        "quantity": 0
    }
]

```







