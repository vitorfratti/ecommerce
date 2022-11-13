<template>
  <div id="app">
    <div class="cart-container" id="cart-container">
      <div class="header-cart-container">
        <h1 v-if="this.cart.length >= 1">Carrinho ({{ this.cart.length }})</h1>
        <h1 v-else>Carrinho</h1>
        <button @click="closeCart">
          <img src="./assets/twoarrows.png" alt="twoarrows" />
        </button>
      </div>
      <div class="main-cart-container">
        <h3 class="empty-text" v-if="this.cart.length <= 0">
          Seu carrinho está vazio.
        </h3>
        <div
          v-else-if="this.cart.length >= 1"
          class="main-cart-container__products-container"
        >
          <div
            v-for="(produto, index) in cart"
            :index="index"
            :key="produto"
            class="productinthecart-container"
          >
            <div class="left">
              <div class="productinthecart__image">
                <img :src="produto.image" :alt="produto.title" />
              </div>
              <div class="productinthecart__infos">
                <h2 class="productinthecart__title">{{ produto.title }}</h2>
                <h2 class="productinthecart__code">
                  Código: {{ produto.code }}
                </h2>
                <h2 class="productinthecart__price">R$ {{ produto.price }}</h2>
              </div>
            </div>
            <div class="right">
              <div class="productinthecart__price-container">
                <h2>R$ {{ produto.price }}</h2>
              </div>
              <div class="productinthecart__delete-container">
                <button @click="deleteFromCart(index)">
                  <img src="./assets/delete.png" alt="delete" />
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-if="this.cart.length >= 1" class="footer-cart-container">
        <div class="footer-cart-container__promocode">
          <div class="footer-cart-container__promocode-input">
            <input
              v-model="promocodeInput"
              type="text"
              placeholder="Código promocional (PROMO123)"
              id="promocode-input"
            />
            <button @click="applyPromocode">Aplicar</button>
          </div>
        </div>
        <div class="footer-cart-container__subtotal">
          <div class="footer-cart-container__subtotal-line">
            <h1>Subtotal</h1>
            <h1>R$ {{ subtotal }}</h1>
          </div>
          <div
            class="footer-cart-container__discount-line none"
            id="footer-cart-container__discount-line"
          >
            <h1>Desconto</h1>
            <h1>(20%) - R$ {{ discount }}</h1>
          </div>
        </div>
        <div class="footer-cart-container__total">
          <h1>Total</h1>
          <h1>R$ {{ total }}</h1>
        </div>
        <div class="footer-cart-container__checkout-btns">
          <button @click="checkoutScreen" class="checkout-btn">Checkout</button>
          <button @click="closeCart" class="continueshopping-btn">
            Continue comprando
          </button>
        </div>
      </div>
    </div>
    <header id="header">
      <button @click="homeScreen" id="home-btn">
        <img src="./assets/apple-logo.png" alt="apple" />
      </button>
      <button @click="openCart" id="opencart-btn">
        <img src="./assets/shopping-bag.png" alt="shopping-bag" />
      </button>
    </header>
    <main>
      <section class="products-container" id="products-container">
        <div class="products-container__filters-container">
          <h3>Ordenar por:</h3>
          <select @change="sortProducts" v-model="filter">
            <option value="Maior Preço">Maior Preço</option>
            <option value="Menor Preço">Menor Preço</option>
          </select>
        </div>
        <div class="products-cards">
          <div
            v-for="(produto, index) in produtos"
            :key="produto"
            :produto="produto"
            @click="openDetails(index)"
            class="product-card"
          >
            <div class="product-card__image">
              <img :src="produto.image" :alt="produto.title" />
            </div>
            <div class="product-card__infos">
              <div class="align-container">
                <h3 class="product-card__title">{{ produto.title }}</h3>
                <h3 class="product-card__price">R$ {{ produto.price }}</h3>
              </div>
            </div>
          </div>
        </div>
      </section>
      <section class="details-container none" id="details-container">
        <div
          v-for="produto in selecionado"
          :key="produto"
          :produto="produto"
          class="details-container__main"
        >
          <div class="details-container__image">
            <img :src="produto.image" :alt="produto.title" />
          </div>
          <div class="details-container__infos">
            <h1 class="details-container__title">{{ produto.title }}</h1>
            <h3 class="details-container__description">
              {{ produto.description }}
            </h3>
            <h1 class="details-container__price">R$ {{ produto.price }}</h1>
            <div class="details-container__inputandbutton">
              <button @click="addToCart" class="addtocart-btn">
                Adicionar ao Carrinho
              </button>
            </div>
          </div>
        </div>
      </section>
      <section class="checkout-container none" id="checkout-container">
        <div class="checkout-container__container">
          <div class="checkout-container__product-images">
            <div
              v-for="produto in cart"
              :key="produto"
              class="checkout-container__image"
            >
              <img :src="produto.image" :alt="produto.title" />
            </div>
          </div>
          <div class="checkout-container__title-container">
            <h2 class="checkout-container__title">Compra efetuada!</h2>
          </div>
          <div class="checkout-container__text-container">
            <h2 v-if="this.cart.length >= 2" class="checkout-container__text">
              Uhuul! Sua compra de {{ this.cart.length }} itens já está a
              caminho da sua casa.
            </h2>
            <h2 v-else class="checkout-container__text">
              Uhuul! Sua compra de {{ this.cart.length }} item já está a caminho
              da sua casa.
            </h2>
          </div>
          <div class="checkout-container__link-container">
            <a @click="backToCatalog()" class="checkout-container__link"
              >Voltar ao catálogo</a
            >
          </div>
        </div>
      </section>
    </main>
    <footer id="footer">
      <div class="pages-btns-container" id="pages-btns-container">
        <button @click="changePages">1</button>
        <button @click="changePages">2</button>
        <button @click="changePages">3</button>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      produtos: [
        {
          title: "iPhone 14 Pro Max Apple, Tela de 6,7'', 5G e Câmera de 48MP",
          price: "9449.10",
          category: "iPhone",
          image: require("./assets/iphone14promax.png"),
          description:
            "Uma nova forma de interação no iPhone. Um recurso essencial de segurança projetado para salvar vidas. Câmera inovadora de 48 MP que revela detalhes impressionantes. Tudo com a potência do chip para smartphone que é o máximo.",
        },
        {
          title:
            "iPhone 13 Pro Max Apple, Tela de 6,1'', 5G e Câmera Tripla de 12MP",
          price: "7599.00",
          category: "iPhone",
          image: require("./assets/iphone13promax.png"),
          description:
            "O maior upgrade do sistema de câmera Pro até hoje. Tela Super Retina xdr com ProMotion para uma experiência mais rápida e responsiva. Chip A15 Bionic com velocidade impressionante. 5G ultrarrápido. Design resistente. E a maior duração de bateria em um iPhone.",
        },
        {
          title:
            "iPhone 12 Pro Max Apple, com Tela de 6,7'', 5G e Câmera Tripla de 12MP",
          price: "7762.73 ",
          category: "iPhone",
          image: require("./assets/iphone12promax.png"),
          description:
            "O A14 Bionic ultrapassa os outros chips de smartphone como um foguete. O sistema de câmera Pro deixa as fotos em pouca luz claramente melhores - uma diferença ainda mais nítida no iPhone 12 Pro Max. O Ceramic Shield é quatro vezes mais duro na queda.",
        },
        {
          title: "Apple AirPods Max Over the Ear (Bluetooth)",
          price: "4299.00",
          category: "AirPods Max",
          image: require("./assets/airpodsmax.png"),
          description:
            "Das almofadas ao arco, os AirPods Max foram pensados para proporcionar um ajuste perfeito, criando uma vedação acústica impecável que leva em consideração diferentes formatos de cabeça.",
        },
        {
          title: "Apple AirPods (3ª Geração) com Estojo de Recarga Lightning",
          price: "1399.00",
          category: "AirPods",
          image: require("./assets/airpods3geracao.png"),
          description:
            "Os AirPods são leves e oferecem ajuste arredondado. Eles se acomodam no ângulo ideal para proporcionar conforto e direcionar melhor o áudio para seus ouvidos. A haste é 33% menor que nos AirPods (2ª geração) e tem um sensor de força que facilita o controle das músicas e chamadas.",
        },
        {
          title:
            "AirTag para iPhone, iPad e iPod Touch com 04 Unidades - Apple",
          price: "1168.74",
          category: "AirTag",
          image: require("./assets/airtags.png"),
          description:
            "Use o AirTag para encontrar suas chaves, carteira, bagagem, mochila e muito mais no app Buscar. Você pode fazer o AirTag emitir um bipe com o alto-falante integrado ou dizer algo como “E aí, Siri, encontra minha mochila”.",
        },
      ],
      selecionado: [],
      cart: [],
      subtotal: 0,
      total: 0,
      discount: 0,
      promocodeInput: null,
      promocode: "PROMO123",
      filter: 'Maior Preço'
    }
  },
  methods: {
    openCart() {
      document.getElementById("cart-container").classList.add("open");
      document.getElementById("header").classList.add("brightness");
      document.getElementById("products-container").classList.add("brightness");
      document.getElementById("footer").classList.add("brightness");
      document.getElementById("details-container").classList.add("brightness");
    },
    closeCart() {
      document.getElementById("cart-container").classList.remove("open");
      document.getElementById("header").classList.remove("brightness");
      document
        .getElementById("products-container")
        .classList.remove("brightness");
      document.getElementById("footer").classList.remove("brightness");
      document
        .getElementById("details-container")
        .classList.remove("brightness");
    },
    openDetails(index) {
      document.getElementById("products-container").classList.add("none");
      document.getElementById("details-container").classList.remove("none");
      document.getElementById("footer").classList.add("none");
      this.selecionado.push({
        title: this.produtos[index].title,
        price: this.produtos[index].price,
        description: this.produtos[index].description,
        image: this.produtos[index].image,
        amount: this.produtos[index].amount,
      });
    },
    addToCart() {
      this.openCart();
      this.cart.push({
        title: this.selecionado[0].title,
        price: this.selecionado[0].price,
        code: Math.floor(Math.random() * 999999) - 1000,
        image: this.selecionado[0].image,
      });
      this.total = (
        parseInt(this.subtotal) + parseInt(this.selecionado[0].price)
      ).toFixed(2);
      this.subtotal = (
        parseInt(this.subtotal) + parseInt(this.selecionado[0].price)
      ).toFixed(2);
      this.homeScreen();
      if (
        document.getElementById("promocode-input").placeholder ==
        "Desconto aplicado ✅"
      ) {
        this.discount = (parseInt(this.subtotal) * 0.2).toFixed(2);
        this.total = (
          parseInt(this.subtotal) - parseInt(this.discount)
        ).toFixed(2);
      }
    },
    deleteFromCart(index) {
      this.total = (
        parseInt(this.subtotal) - parseInt(this.cart[index].price)
      ).toFixed(2);
      this.subtotal = (
        parseInt(this.subtotal) - parseInt(this.cart[index].price)
      ).toFixed(2);
      this.cart.splice(index, 1);
      if (this.cart <= 0) {
        this.closeCart();
        this.discount = 0;
      }
      if (
        document.getElementById("promocode-input").placeholder ==
        "Desconto aplicado ✅"
      ) {
        this.discount = (parseInt(this.subtotal) * 0.2).toFixed(2);
        this.total = (
          parseInt(this.subtotal) - parseInt(this.discount)
        ).toFixed(2);
      }
    },
    homeScreen() {
      document.getElementById("header").classList.remove("center");
      document.getElementById("opencart-btn").classList.remove("none");
      document.getElementById("checkout-container").classList.add("none");
      document.getElementById("products-container").classList.remove("none");
      document.getElementById("details-container").classList.add("none");
      document.getElementById("footer").classList.remove("none");
      this.selecionado = [];
      if (document.getElementById("home-btn").value == 1) {
        this.cart = [];
        this.subtotal = 0;
        this.total = 0;
      }
      document.getElementById("home-btn").value = 0;
    },
    applyPromocode() {
      if (this.promocodeInput == this.promocode) {
        document
          .getElementById("footer-cart-container__discount-line")
          .classList.remove("none");
        this.discount = (parseInt(this.subtotal) * 0.2).toFixed(2);
        this.total = (
          parseInt(this.subtotal) - parseInt(this.discount)
        ).toFixed(2);
        document.getElementById("promocode-input").placeholder =
          "Desconto aplicado ✅";
        this.promocodeInput = null;
      }
    },
    checkoutScreen() {
      document.getElementById("checkout-container").classList.remove("none");
      document.getElementById("products-container").classList.add("none");
      document.getElementById("details-container").classList.add("none");
      document.getElementById("footer").classList.add("none");
      document.getElementById("header").classList.add("center");
      document.getElementById("opencart-btn").classList.add("none");
      document.getElementById("home-btn").value = 1;
      this.closeCart();
    },
    backToCatalog() {
      this.homeScreen();
      this.cart = [];
      this.subtotal = 0;
      this.total = 0;
    },
    changePages() {
      alert("Sem mais produtos...");
    },
    sortProducts() {
      if(this.filter === 'Menor Preço') {
        this.produtos.sort((a, b) => a.price - b.price)
      } else if(this.filter === 'Maior Preço') {
        this.produtos.sort((a, b) => b.price - a.price)
      }
    }
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@100;200;300;400;500;600;700;800;900&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Inter", sans-serif;
}

::-webkit-scrollbar {
  width: 0.5rem;
}

body {
  background: #ffffff;
}

header {
  background: #ffffff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 8rem;
  padding: 0 10%;
  transition: 0.4s;

  button {
    background: none;
    border-radius: 6px;
    border: none;
    cursor: pointer;
    padding: 0.25rem;
    transition: 0.4s;

    img {
      width: 3rem;
    }
  }
}

main {
  background: #ffffff;

  section.products-container {
    padding: 2rem 10% 4rem 10%;
    height: 100%;
    transition: 0.4s;
    background: #ffffff;

    .products-container__filters-container {
      display: flex;
      justify-content: flex-start;
      align-items: center;
      flex-wrap: wrap;
      width: 100%;
      padding: 0 1.5rem;

      h3 {
        font-size: 1rem;
        font-weight: 500;
        color: #171717;
      }

      select {
        padding: 0.2rem;
        margin-left: 1rem;
        border: 1px solid #3b3b3b;
        border-radius: 4px;
      }
    }

    .products-cards {
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      padding-top: 2rem;

      .product-card {
        background: #f6f6f6;
        width: 28rem;
        height: 28rem;
        border-radius: 6px;
        margin: 1.5rem;
        cursor: pointer;
        transition: 0.2s;
        flex-basis: 15rem;
        flex-grow: 1;

        &:hover {
          transform: scale(1.01);
          transition: 0.2s;
        }

        .product-card__image {
          display: flex;
          justify-content: center;
          align-items: center;
          height: 70%;
          background: #f6f6f6;
          box-shadow: inset 0 0 2px #dadada;

          img {
            transform: scale(0.6);
          }
        }

        .product-card__infos {
          display: flex;
          justify-content: space-between;
          align-items: center;
          height: 30%;
          background: #ffffff;

          h3.product-card__title {
            font-size: 1.1rem;
            font-weight: 400;
            color: #171717;
            margin-bottom: 0.5rem;
          }

          h3.product-card__price {
            font-size: 1.1rem;
            font-weight: 600;
            color: #000;
          }
        }
      }
    }
  }

  section.details-container {
    transition: 0.4s;
    background: #ffffff;
    height: calc(100vh - 8rem);

    .details-container__main {
      display: flex;
      justify-content: space-between;
      padding: 2rem 10rem 4rem 10rem;
      height: calc(100vh - 16rem);
      transition: 0.4s;
      background: #ffffff;

      .details-container__image {
        width: 49%;
        height: 100%;
        background: #f6f6f6;
        box-shadow: inset 0 0 2px #dadada;
        display: flex;
        justify-content: center;
        align-items: center;
      }

      .details-container__infos {
        width: 46%;
        height: 100%;

        h1.details-container__title {
          color: #222222;
          font-weight: 600;
          font-size: 2.5rem;
        }

        h1.details-container__price {
          color: #222222;
          font-size: 2rem;
          font-weight: 600;
          margin-top: 1.5rem;
        }

        h3.details-container__description {
          color: #000;
          font-size: 1.2rem;
          font-weight: 300;
          margin-top: 1.5rem;
        }

        .details-container__inputandbutton {
          display: flex;
          justify-content: space-between;
          align-items: center;
          margin-top: 2rem;

          input {
            width: 10%;
            height: 4rem;
            border: 1px solid #535353;
            font-size: 1.3rem;
            padding: 0 0.5rem;
            border-radius: 4px;
          }

          button.addtocart-btn {
            background: #0071e3;
            color: #ffffff;
            font-size: 1.1rem;
            width: 88%;
            height: 4rem;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: 0.4s;
          }
        }
      }
    }
  }
}

section.checkout-container {
  padding: 0 2rem;
  height: calc(100vh - 20rem);
  transition: 0.4s;
  background: #ffffff;
  display: flex;
  justify-content: center;
  align-items: center;

  .checkout-container__product-images {
    display: flex;
    justify-content: center;
    align-items: center;

    div {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-wrap: wrap;
      background: #ffffff;
      width: 8rem;
      height: 8rem;
      border-radius: 50%;
      box-shadow: 0 0 30px #b9b9b9;
      z-index: 10;
      margin: 0 -1.5rem;

      img {
        width: 6rem;
      }
    }
  }

  .checkout-container__title-container {
    display: flex;
    justify-content: center;
    margin-top: 3rem;

    h2.checkout-container__title {
      color: #171717;
      font-size: 2rem;
      font-weight: 800;
      text-align: center;
    }
  }

  .checkout-container__text-container {
    display: flex;
    justify-content: center;
    margin-top: 1.5rem;

    h2.checkout-container__text {
      color: #171717;
      font-size: 1.5rem;
      font-weight: 600;
      text-align: center;
    }
  }

  .checkout-container__link-container {
    display: flex;
    justify-content: center;
    margin-top: 4rem;

    a.checkout-container__link {
      color: #171717;
      font-size: 1.2rem;
      font-weight: 600;
      text-decoration: underline;
      cursor: pointer;
      transition: 0.4s;
      text-align: center;

      &:hover {
        transition: 0.4s;
        opacity: 0.7;
      }
    }
  }
}

footer {
  display: flex;
  justify-content: center;
  padding: 0 0 3rem 0;
  height: 8rem;
  background: #ffffff;
  transition: 0.4s;

  .pages-btns-container {
    display: flex;
    justify-content: space-between;
    width: 10rem;
    background: #ffffff;

    button {
      background: #f6f6f6;
      border: 1px solid #dadada;
      width: 30%;
      height: 3rem;
      font-size: 1.2rem;
      color: #171717;
      border-radius: 3px;
      cursor: pointer;
    }
  }
}

.cart-container {
  position: fixed;
  z-index: 10;
  right: -45rem;
  width: 45rem;
  height: 100vh;
  background: #ffffff;
  transition: 0.4s;
  box-shadow: 0 0 20px #9b9b9b;

  .header-cart-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 2rem;
    background: #ffffff;
    border-bottom: 1px solid #f0f0f0;

    h1 {
      color: #171717;
    }

    button {
      background: none;
      border: none;
      cursor: pointer;

      img {
        width: 2rem;
      }
    }
  }

  .main-cart-container {
    background: #ffffff;
    padding: 1rem 2rem;
    height: calc(100vh - 30rem);
    overflow-y: scroll;

    .productinthecart-container {
      display: flex;
      justify-content: space-between;
      align-items: center;
      height: 6rem;
      margin-bottom: 1rem;

      .left {
        width: 55%;
        display: flex;
        justify-content: space-between;
        align-items: center;

        .productinthecart__image {
          display: flex;
          justify-content: center;
          align-items: center;
          width: 5rem;
          height: 5rem;
          background: #f6f6f6;
          border: 1px solid #dadada;
          border-radius: 6px;

          img {
            width: 4.5rem;
          }
        }

        .productinthecart__infos {
          width: 16rem;

          h2.productinthecart__title {
            color: #171717;
            font-size: 1rem;
            font-weight: 500;
          }

          h2.productinthecart__code {
            color: #171717;
            font-size: 1rem;
            font-weight: 300;
          }

          h2.productinthecart__price {
            display: none;
            color: #171717;
            font-size: 1rem;
            font-weight: 500;
          }
        }
      }

      .right {
        width: 30%;
        display: flex;
        justify-content: space-between;

        .productinthecart__amount {
          display: flex;
          justify-content: center;
          border: 1px solid #dadada;
          border-radius: 10px;
          align-items: center;

          button {
            border: none;
            background: #ffffff;
            width: 2rem;
            height: 2rem;
            font-size: 1.2rem;
            cursor: pointer;
            border-radius: 50%;
            color: #171717;
          }

          h3 {
            background: #ffffff;
            font-size: 1rem;
            color: #171717;
          }
        }

        .productinthecart__price-container {
          h2 {
            color: #171717;
            font-size: 1.2rem;
            font-weight: 500;
          }
        }

        .productinthecart__delete-container {
          button {
            background: none;
            border: none;
            cursor: pointer;

            img {
              width: 1.5rem;
            }
          }
        }
      }
    }
  }

  .footer-cart-container {
    border-top: 1px solid #f0f0f0;
    padding: 1.5rem 2rem;

    .footer-cart-container__subtotal {
      padding-top: 1rem;

      .footer-cart-container__subtotal-line {
        display: flex;
        justify-content: space-between;
        align-items: center;

        h1 {
          color: #171717;
          font-size: 1.2rem;
          font-weight: 400;
        }
      }

      .footer-cart-container__discount-line {
        display: flex;
        justify-content: space-between;
        align-items: center;

        h1 {
          color: #535353;
          font-size: 0.9rem;
          font-weight: 300;
        }
      }
    }

    .footer-cart-container__total {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 1rem 0;
      margin-top: 1rem;
      border-top: 1px solid #f0f0f0;

      h1 {
        color: #171717;
        font-size: 1.2rem;
        font-weight: 600;
      }
    }

    .footer-cart-container__checkout-btns {
      margin-top: 0.5rem;

      button.checkout-btn {
        background: #0071e3;
        color: #ffffff;
        padding: 1rem;
        border: none;
        cursor: pointer;
        font-size: 1rem;
        border-radius: 8px;
        width: 100%;
        transition: 0.4s;
      }

      button.continueshopping-btn {
        background: #ffffff;
        color: #171717;
        padding: 1rem;
        border: none;
        box-shadow: 0 0 3px #b8b8b8;
        cursor: pointer;
        font-size: 1rem;
        border-radius: 8px;
        width: 100%;
        margin-top: 0.5rem;
        transition: 0.4s;
      }
    }

    .footer-cart-container__promocode {
      padding: 0rem 0 1.5rem 0;
      border-bottom: 1px solid #f0f0f0;

      .footer-cart-container__promocode-input {
        display: flex;
        justify-content: space-between;
        align-items: center;
        width: 100%;
        height: 3rem;
        box-shadow: 0 0 2px #b8b8b8;
        border-radius: 10px;

        input {
          border: none;
          width: 80%;
          height: 3rem;
          border-radius: 10px;
          padding-left: 0.7rem;
          font-size: 1.2rem;

          &:focus {
            outline: none;
          }
        }

        button {
          font-size: 1.2rem;
          border: 1px solid #dadada;
          height: 3rem;
          width: 5rem;
          background: #ffffff;
          border-radius: 10px;
          cursor: pointer;
        }
      }
    }
  }
}

h3.empty-text {
  margin-top: 4rem;
  text-align: center;
  font-weight: 400;
}

button:hover {
  opacity: 0.8;
  transition: 0.4s;
}

.open {
  right: 0 !important;
  transition: 0.4s;
}

.none {
  display: none !important;
  transition: 0.4s;
}

.center {
  justify-content: center !important;
}

.brightness {
  filter: brightness(70%) !important;
  transition: 0.4s;
}

@media only screen and (max-width: 1150px) {

  .products-container__filters-container {
    padding: 0 !important;
  }

  .product-card {
    margin: 1.5rem 0 !important;
  }

  .details-container__infos {
    h1 {
      font-size: 2rem !important;
    }

    h3 {
      font-size: 1.2rem !important;
    }
  }

  .details-container__image {
    height: 35rem !important;

    img {
      width: 20rem !important;
    }
  }
}

@media only screen and (max-width: 1000px) {
  .details-container__main {
    display: block !important;
  }

  .details-container__image {
    width: 100% !important;
    height: 16rem !important;

    img {
      width: 16rem !important;
    }
  }

  .details-container__infos {
    margin-top: 1rem !important;
    width: 100% !important;
    height: 20rem !important;

    h3 {
      font-size: 1.1rem !important;
    }

    button {
      width: 100% !important;
    }
  }
}

@media only screen and (max-width: 750px) {

  .details-container__infos {
    h1 {
      font-size: 1.3rem !important;
    }

    h3 {
      font-size: 0.9rem !important;
    }
  }

  .cart-container {
    width: 100% !important;
    right: -60rem;
  }

  .productinthecart-container {
    .left {
      width: 80% !important;
      justify-content: flex-start !important;
    }

    .right {
      width: 10% !important;
      justify-content: flex-end !important;
    }

    .productinthecart__infos {
      display: grid !important;
      margin-left: 1rem !important;
    }

    h2.productinthecart__price {
      display: block !important;
    }

    .productinthecart__price-container {
      display: none !important;
    }
  }
}

@media only screen and (max-width: 560px) {
  .product-card {
    width: 100% !important;

    .product-card__image {
      img {
        transform: scale(0.5) !important;
      }
    }
  }

  .details-container__main {
    padding: 2rem 3rem 4rem 3rem !important;
    height: calc(100vh - 10rem) !important;
  }

  .details-container__infos {
    h1 {
      font-size: 1.5rem !important;
    }

    h3 {
      font-size: 1rem !important;
    }
  }
}

@media only screen and (max-width: 500px) {

  .header-cart-container {
    h1 {
      font-size: 1.5rem !important;
    }

    button {
      img {
        width: 1.5rem !important;
      }
    }
  }

  .details-container__main {
    height: 100vh !important;
  }

  .productinthecart-container {
    height: 7rem !important;
  }

  .productinthecart__infos {
    h2 {
      font-size: 0.8rem !important;
    }
  }

  .footer-cart-container {
    button {
      font-size: 1rem !important;
    }

    input {
      font-size: 1rem !important;
    }

    h1 {
      font-size: 1rem !important;
    }
  }
}

@media only screen and (max-width: 350px) {

  header {
    button {
      img {
        width: 2rem !important;
      }
    }
  }

  .productinthecart__infos {
    h2 {
      font-size: 0.7rem !important;
    }
  }

  .product-card {
    width: 15rem !important;

    .product-card__image {
      img {
        transform: scale(0.4) !important;
      }
    }
  }

  .details-container__image {
    img {
      transform: scale(0.7) !important;
    }
  }

  .details-container__infos {
    h1 {
      font-size: 1.2rem !important;
    }

    h3 {
      font-size: 0.9rem !important;
    }
  }

  .details-container__inputandbutton {
    button {
      font-size: 0.9rem !important;
    }
  }
}

</style>