describe('Test', () => {
  const username = 'standard_user';
  const password = 'secret_sauce';
  const firstName = 'User';
  const lastName = 'User';
  const postalCode = 426060;

  beforeEach(() => {
    cy.visit('https://www.saucedemo.com');
  });

  it('should allow successful login', () => {
    cy.get('#user-name').type(username).should('have.value', username);
    cy.get('#password').type(password).should('have.value', password);
    cy.get('#login-button').click();
    cy.url().should('include', '/inventory.html');
  });

  it('sort', () => {
    cy.login(username, password);

    const verifySortedPrices = (sortDirection) => {
      let lastPrice = 0.0;
      cy.get('.product_sort_container').select(sortDirection);

      cy.get('.inventory_item[data-test = "inventory-item"]').each((el) => {
        const price = parseFloat(el.find('.inventory_item_price').text().replace('$', ''));
        if (sortDirection === 'lohi') {
          expect(price).to.be.greaterThanOrEqual(lastPrice);
        } else {
          expect(price).to.be.lessThanOrEqual(lastPrice);
        }
        lastPrice = price;
      });
    };
    verifySortedPrices('lohi');
    verifySortedPrices('hilo');
  });

  it('should allow adding products to cart and checkout', () => {
    cy.login(username, password);

    cy.addProductsToCart(0, 1);
    cy.url().should('eq', 'https://www.saucedemo.com/cart.html');
    cy.get('.cart_list[data-test = "cart-list"]').children('.cart_item').should('have.length', 2);

    cy.get('.btn[data-test = "checkout"]').click();
    cy.fillCheckoutForm(firstName, lastName, postalCode);
    cy.get('.btn[data-test="finish"]').click();
    cy.url().should('include', '/checkout-complete.html');
    cy.get('.btn[data-test="back-to-products"]').click();
    cy.url().should('include', '/inventory.html');
  });

  cy.login = (user, pass) => {
    cy.get('#user-name').type(user).should('have.value', user);
    cy.get('#password').type(pass).should('have.value', pass);
    cy.get('#login-button').click();
  };

  cy.addProductsToCart = (index1, index2) => {
    cy.get('.inventory_item[data-test="inventory-item"]')
      .eq(index1)
      .find('.btn[data-test = "add-to-cart-sauce-labs-backpack"]')
      .click();
    cy.get('.inventory_item[data-test="inventory-item"]')
      .eq(index2)
      .find('.btn[data-test = "add-to-cart-sauce-labs-bike-light"]')
      .click();
  };

  cy.fillCheckoutForm = (first, last, code) => {
    cy.get('.form_input[data-test="firstName"]').type(first).should('have.value', first);
    cy.get('.form_input[data-test="lastName"]').type(last).should('have.value', last);
    cy.get('.form_input[data-test="postalCode"]').type(code).should('have.value', code);
    cy.get('.submit-button[data-test = "continue"]').click();
  };
});
