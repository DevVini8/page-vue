<template>
  <div class="store-layout">
    <ToastNotification :toasts="toasts" @dismiss="removeToast" />

    <header class="store-header">
      <div class="container header-bar">
        <div class="brand">
          <h1>TechStore</h1>
          <p>Equipamentos e acessórios com envio rápido</p>
        </div>

        <div class="cart-summary">
          <div class="cart-info">
            <span>Carrinho ({{ cartCount }} {{ cartCount === 1 ? 'item' : 'itens' }})</span>
            <strong>R$ {{ cartTotal.toFixed(2) }}</strong>
          </div>
          <div class="cart-buttons">
            <button
              type="button"
              class="btn btn-light"
              :disabled="cartCount === 0"
              @click="clearCart"
            >
              Limpar
            </button>
            <button
              type="button"
              class="btn btn-checkout"
              @click="checkout"
            >
              Finalizar Compra
            </button>
          </div>
        </div>
      </div>
    </header>

    <main class="container store-main">
      <section class="shipping-banner">
        <ProgressBar
          :progress="shippingProgress"
          :label="shippingMessage"
        />
      </section>

      <section class="catalog-section">
        <div class="catalog-toolbar">
          <h2>Produtos em Destaque</h2>
          <DropdownMenu
            label="Filtrar por Categoria"
            :options="categoryOptions"
            @select="handleCategorySelect"
          />
        </div>

        <div class="products-grid">
          <article
            v-for="product in paginatedProducts"
            :key="product.id"
            class="product-card"
          >
            <span class="product-tag">{{ product.category }}</span>
            <h3 class="product-title">{{ product.name }}</h3>
            <p class="product-description">{{ product.description }}</p>
            <div class="product-bottom">
              <span class="product-price">R$ {{ product.price.toFixed(2) }}</span>
              <button
                type="button"
                class="btn btn-primary"
                @click="addToCart(product)"
              >
                Comprar
              </button>
            </div>
          </article>
        </div>

        <Pagination
          :totalPages="totalPages"
          :currentPage="currentPage"
          @change-page="handlePageChange"
        />
      </section>

      <section class="info-section">
        <h2>Informações da Loja</h2>
        <Tabs :tabs="storeTabs" @tab-change="handleTabChange" />
      </section>

      <section class="faq-section">
        <h2>Dúvidas Frequentes</h2>
        <Accordion :items="faqItems" />
      </section>
    </main>

    <footer class="store-footer">
      <div class="container">
        <p>TechStore · Todos os direitos reservados</p>
      </div>
    </footer>
  </div>
</template>

<script>
import Accordion from './components/Accordion.vue'
import Pagination from './components/Pagination.vue'
import ProgressBar from './components/ProgressBar.vue'
import Tabs from './components/Tabs.vue'
import DropdownMenu from './components/DropdownMenu.vue'
import ToastNotification from './components/ToastNotification.vue'

export default {
  name: 'App',
  components: {
    Accordion,
    Pagination,
    ProgressBar,
    Tabs,
    DropdownMenu,
    ToastNotification
  },
  data() {
    return {
      cart: [],
      freeShippingGoal: 1000,
      currentPage: 1,
      itemsPerPage: 3,
      selectedCategory: 'all',
      toasts: [],
      categoryOptions: [
        { label: 'Todas as Categorias', value: 'all' },
        { label: 'Periféricos', value: 'Periféricos' },
        { label: 'Hardware', value: 'Hardware' },
        { label: 'Monitores', value: 'Monitores' }
      ],
      products: [
        {
          id: 1,
          name: 'Teclado Mecânico Compacto',
          category: 'Periféricos',
          description: 'Switches silenciosos, iluminação branca e cabo USB-C destacável.',
          price: 299.90
        },
        {
          id: 2,
          name: 'Mouse Sem Fio Ultraleve',
          category: 'Periféricos',
          description: 'Sensor óptico de alta precisão e bateria para até 70 horas de uso.',
          price: 199.90
        },
        {
          id: 3,
          name: 'Monitor IPS 24" Full HD',
          category: 'Monitores',
          description: 'Bordas ultrafinas, 75Hz e filtro de luz azul para maior conforto visual.',
          price: 749.00
        },
        {
          id: 4,
          name: 'SSD NVMe 500GB',
          category: 'Hardware',
          description: 'Velocidade de leitura de 3500 MB/s para iniciar o sistema em segundos.',
          price: 289.00
        },
        {
          id: 5,
          name: 'Headset com Microfone',
          category: 'Periféricos',
          description: 'Almofadas macias em espuma e áudio estéreo nítido para chamadas e jogos.',
          price: 229.90
        },
        {
          id: 6,
          name: 'Memória RAM 16GB DDR4',
          category: 'Hardware',
          description: 'Frequência de 3200MHz com dissipador em alumínio para alto desempenho.',
          price: 259.00
        },
        {
          id: 7,
          name: 'Monitor Gamer 27" 144Hz',
          category: 'Monitores',
          description: 'Tempo de resposta de 1ms e suporte com ajuste de inclinação.',
          price: 1199.00
        },
        {
          id: 8,
          name: 'Webcam Full HD 1080p',
          category: 'Periféricos',
          description: 'Correção automática de luz e microfone embutido com redução de ruído.',
          price: 179.90
        },
        {
          id: 9,
          name: 'Fonte 650W 80 Plus Bronze',
          category: 'Hardware',
          description: 'Cabos revestidos, ventoinha silenciosa e proteção contra surtos.',
          price: 349.00
        }
      ],
      storeTabs: [
        {
          title: 'Formas de Pagamento',
          heading: 'Opções simples e seguras para sua compra',
          content: 'Aceitamos pagamentos com aprovação imediata e parcelamento sem juros no cartão.',
          details: [
            { label: 'PIX', value: '10% de desconto na hora' },
            { label: 'Cartão de Crédito', value: 'Em até 10x sem juros' },
            { label: 'Boleto Bancário', value: 'Compensação em até 1 dia útil' }
          ]
        },
        {
          title: 'Envio e Entrega',
          heading: 'Como seu pedido chega até você',
          content: 'Todos os produtos em estoque são despachados em até 24 horas úteis após a confirmação.',
          details: [
            { label: 'Frete Grátis', value: 'Compras acima de R$ 1.000,00' },
            { label: 'Rastreamento', value: 'Código enviado por e-mail' },
            { label: 'Embalagem', value: 'Proteção reforçada anti-impacto' }
          ]
        },
        {
          title: 'Garantia e Trocas',
          heading: 'Compre com total tranquilidade',
          content: 'Caso precise trocar ou devolver um produto, o processo é rápido e sem custos adicionais.',
          details: [
            { label: 'Devolução Grátis', value: 'Até 7 dias após o recebimento' },
            { label: 'Garantia de Fábrica', value: '12 meses em todos os itens' },
            { label: 'Suporte', value: 'Atendimento de segunda a sábado' }
          ]
        }
      ],
      faqItems: [
        {
          title: 'Como funciona a meta de Frete Grátis?',
          content: 'Basta adicionar produtos ao carrinho. Quando o valor total atingir R$ 1.000,00, a barra de progresso chega a 100% e o frete grátis é liberado automaticamente.'
        },
        {
          title: 'Posso acompanhar a entrega do meu pedido?',
          content: 'Sim. Assim que o pedido é faturado, enviamos o código de rastreio para que você acompanhe cada etapa da entrega.'
        },
        {
          title: 'Os produtos possuem nota fiscal e garantia?',
          content: 'Todos os produtos vendidos acompanham Nota Fiscal eletrônica e possuem 1 ano de garantia direta.'
        }
      ]
    }
  },
  computed: {
    cartCount() {
      return this.cart.length
    },
    cartTotal() {
      return this.cart.reduce((sum, item) => sum + item.price, 0)
    },
    shippingProgress() {
      return Math.min(100, Math.round((this.cartTotal / this.freeShippingGoal) * 100))
    },
    shippingMessage() {
      if (this.cartTotal >= this.freeShippingGoal) {
        return 'Parabéns! Você ganhou Frete Grátis na sua compra!'
      }
      const remaining = this.freeShippingGoal - this.cartTotal
      return 'Faltam R$ ' + remaining.toFixed(2) + ' para você ganhar Frete Grátis'
    },
    filteredProducts() {
      if (this.selectedCategory === 'all') {
        return this.products
      }
      return this.products.filter(product => product.category === this.selectedCategory)
    },
    totalPages() {
      return Math.max(1, Math.ceil(this.filteredProducts.length / this.itemsPerPage))
    },
    paginatedProducts() {
      const start = (this.currentPage - 1) * this.itemsPerPage
      return this.filteredProducts.slice(start, start + this.itemsPerPage)
    }
  },
  methods: {
    addToast(message, type = 'info') {
      const id = Date.now() + Math.random()
      this.toasts.push({ id, message, type })
      setTimeout(() => {
        this.removeToast(id)
      }, 3500)
    },
    removeToast(id) {
      this.toasts = this.toasts.filter(toast => toast.id !== id)
    },
    addToCart(product) {
      const hadFreeShipping = this.cartTotal >= this.freeShippingGoal
      this.cart.push(product)
      if (!hadFreeShipping && this.cartTotal >= this.freeShippingGoal) {
        this.addToast('Você atingiu R$ 1.000,00 e desbloqueou Frete Grátis!', 'success')
      } else {
        this.addToast(product.name + ' adicionado ao carrinho!', 'success')
      }
    },
    clearCart() {
      this.cart = []
      this.addToast('O carrinho foi esvaziado.', 'info')
    },
    checkout() {
      if (this.cart.length === 0) {
        this.addToast('Adicione pelo menos um produto antes de finalizar a compra.', 'error')
        return
      }
      this.cart = []
      this.addToast('Pedido realizado com sucesso! Obrigado pela compra.', 'success')
    },
    handlePageChange(page) {
      this.currentPage = page
    },
    handleCategorySelect(option) {
      this.selectedCategory = option.value
      this.currentPage = 1
      this.addToast('Exibindo categoria: ' + option.label, 'info')
    },
    handleTabChange(tab) {
      this.addToast('Visualizando: ' + tab.title, 'info')
    }
  },
  mounted() {
    this.addToast('Bem-vindo à TechStore! Confira nossas ofertas.', 'info')
  }
}
</script>

<style scoped>
.store-layout {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.container {
  width: 100%;
  max-width: 1100px;
  margin: 0 auto;
  padding: 0 1.25rem;
}

.store-header {
  background-color: #1e293b;
  color: #ffffff;
  padding: 1.5rem 0;
}

.header-bar {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}

.brand h1 {
  font-size: 1.65rem;
  font-weight: 800;
  letter-spacing: -0.02em;
}

.brand p {
  font-size: 0.9rem;
  color: #94a3b8;
}

.cart-summary {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 1rem;
  background-color: #0f172a;
  padding: 0.75rem 1rem;
  border-radius: 0.5rem;
}

.cart-info {
  display: flex;
  flex-direction: column;
  font-size: 0.85rem;
  color: #cbd5e1;
}

.cart-info strong {
  font-size: 1.1rem;
  color: #ffffff;
}

.cart-buttons {
  display: flex;
  gap: 0.5rem;
}

.store-main {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 2rem;
  padding-top: 1.75rem;
  padding-bottom: 3rem;
}

.shipping-banner,
.catalog-section,
.info-section,
.faq-section {
  background-color: #ffffff;
  border: 1px solid #e2e8f0;
  border-radius: 0.75rem;
  padding: 1.5rem;
}

.catalog-toolbar {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
  margin-bottom: 1.25rem;
}

.catalog-toolbar h2,
.info-section h2,
.faq-section h2 {
  font-size: 1.25rem;
  color: #0f172a;
}

.info-section h2,
.faq-section h2 {
  margin-bottom: 1rem;
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.25rem;
}

.product-card {
  border: 1px solid #e2e8f0;
  border-radius: 0.5rem;
  padding: 1.25rem;
  background-color: #f8fafc;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.product-tag {
  font-size: 0.75rem;
  font-weight: 700;
  color: #2563eb;
  text-transform: uppercase;
}

.product-title {
  font-size: 1.05rem;
  color: #1e293b;
  margin: 0.35rem 0 0.5rem 0;
}

.product-description {
  font-size: 0.875rem;
  color: #64748b;
  margin-bottom: 1.25rem;
  flex: 1;
}

.product-bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 0.75rem;
  border-top: 1px solid #e2e8f0;
}

.product-price {
  font-size: 1.1rem;
  font-weight: 700;
  color: #0f172a;
}

.btn {
  padding: 0.55rem 1rem;
  border-radius: 0.375rem;
  font-size: 0.875rem;
  font-weight: 600;
  border: none;
  cursor: pointer;
  transition: background-color 0.2s ease, opacity 0.2s ease;
}

.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.btn-primary {
  background-color: #2563eb;
  color: #ffffff;
}

.btn-primary:hover:not(:disabled) {
  background-color: #1d4ed8;
}

.btn-checkout {
  background-color: #10b981;
  color: #ffffff;
}

.btn-checkout:hover:not(:disabled) {
  background-color: #059669;
}

.btn-light {
  background-color: #334155;
  color: #f8fafc;
}

.btn-light:hover:not(:disabled) {
  background-color: #475569;
}

.store-footer {
  background-color: #0f172a;
  color: #94a3b8;
  text-align: center;
  padding: 1.25rem 0;
  font-size: 0.85rem;
}

@media (max-width: 850px) {
  .products-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 580px) {
  .products-grid {
    grid-template-columns: 1fr;
  }

  .cart-summary {
    width: 100%;
    justify-content: space-between;
  }
}
</style>
