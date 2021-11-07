# Playwright vs Cypress

Созданы для автоматизации `end-to-end` тестирования.

## [Playwright](https://github.com/dromanenko/itmo-testing/client/src/tests/e2e)

- Playwright работает в Webkit-браузерах, Cypress - нет;
- Не создает файлов;
- Playwright позволяет тестировать одновременно в нескольких браузерах;
- Можно запустить несколько браузеров, используя один и тот же тест;
- Cypress по умолчанию не работает в безголовом режиме, в отличие от Playwright;
- Playwright позволяет расположить тесты в любом месте;
- Необходимо использовать `async`/`await` в тестах;
- Playwright ожидает UI-элементов перед запуском взаимодействий, Cypress повторно проверяет утверждения до истечения времени ожидания.

### Пример
```javascript
test.describe('Recipes', () => {
    test('Add Recipe', async ({page}) => {
        const random = Math.random()
        const login = 'login' + random
        const password = 'password' + random
        await page.goto('http://localhost:3000/register')
        await submit(page, login, password)
        const name = 'name'
        const description = 'description'
        await addRecipe(page, name + random, description + random)
        await expect(await page.locator('text=' + name + random)).toHaveText(name + random)
        await expect(await page.locator('text=' + description + random)).toHaveText(description + random)
    })
})
```


## [Cypress](https://github.com/dromanenko/itmo-testing/client/cypress/integration/Main.spec.js)

- Тесты необходимо расположить в `<app>/cypress/integration/`;
- Создает несколько файлов и папок с примерами;
- Нет нужды в `async`/`await` в тестах;
- Необходимо повторно запустить тесты для запуска в другом браузере;
- Более обширный и с большим количеством зависимостей;
- Приятный формат вывода.

### Пример
```javascript
describe('Main', () => {
    it('Register', () => {
        const user = 'user' +  Math.random()
        cy.visit('http://localhost:3000')
        cy.contains('Register').click()
        cy.get('[name=\'login\']').type(user)
        cy.get('[name=\'password\']').type('password')
        cy.contains('Sign up').click()
        cy.get('h1').should('have.text', 'Hello, ' + user + '!These are your favourite recipes!')
    })
})
```