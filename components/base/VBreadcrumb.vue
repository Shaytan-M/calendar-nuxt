<script setup lang="ts">
export type VBreadcrumbSeparator = 'arrow' | 'bullet' | 'dot' | 'succeeds'
export type VBreadcrumbAlign = 'center' | 'right'
export interface VBreadcrumbItem {
  label: string
  hideLabel?: boolean
  icon?: string
  link?: string
  to?: any
}
export interface VBreadcrumbsProps {
  items: VBreadcrumbItem[]
  separator?: VBreadcrumbSeparator
  align?: VBreadcrumbAlign
  withIcons?: boolean
}

const props = withDefaults(defineProps<VBreadcrumbsProps>(), {
  separator: undefined,
  align: undefined,
})
</script>

<template>
  <nav
    role="navigation"
    class="breadcrumb"
    aria-label="breadcrumbs"
    itemscope
    itemtype="https://schema.org/BreadcrumbList"
    :class="[`has-${props.separator}-separator`, props.align && `is-${props.align}`]"
  >
    <ul>
      <li
        v-for="(item, key) in props.items"
        :key="key"
        :aria-current="key === items.length - 1 ? 'page' : undefined"
        itemprop="itemListElement"
        itemscope
        itemtype="https://schema.org/ListItem"
      >
        <slot
          name="breadcrumb-item"
          :item="item"
          :index="key"
        >
          <NuxtLink
            v-if="item.to"
            class="breadcrumb-item"
            itemprop="item"
            :to="item.to"
          >
            <span
              v-if="props.withIcons && !!item.icon"
              class="icon is-small"
              :class="[item.hideLabel && props.withIcons && !!item.icon && 'is-solo']"
            >
              <VIcon :icon="item.icon" />
            </span>
            <meta
              v-if="item.hideLabel && props.withIcons && !!item.icon"
              itemprop="name"
              :content="item.label"
            >
            <span
              v-else
              itemprop="name"
            >
              <slot
                name="breadcrumb-item-label"
                :item="item"
                :index="key"
              >
                {{ item.label }}
              </slot>
            </span>

            <meta
              itemprop="position"
              :content="`${key + 1}`"
            >
          </NuxtLink>
          <a
            v-else-if="item.link"
            class="breadcrumb-item"
            itemprop="item"
            :href="item.link"
          >
            <span
              v-if="props.withIcons && !!item.icon"
              class="icon is-small"
              :class="[item.hideLabel && props.withIcons && !!item.icon && 'is-solo']"
            >
              <VIcon :icon="item.icon" />
            </span>
            <meta
              v-if="item.hideLabel && props.withIcons && !!item.icon"
              itemprop="name"
              :content="item.label"
            >
            <span
              v-else
              itemprop="name"
            >
              <slot
                name="breadcrumb-item-label"
                :item="item"
                :index="key"
              >
                {{ item.label }}
              </slot>
            </span>

            <meta
              itemprop="position"
              :content="`${key + 1}`"
            >
          </a>
          <span
            v-else
            class="breadcrumb-item"
          >
            <span
              v-if="props.withIcons && !!item.icon"
              class="icon is-small"
              :class="[item.hideLabel && props.withIcons && !!item.icon && 'is-solo']"
            >
              <VIcon :icon="item.icon" />
            </span>
            <meta
              v-if="item.hideLabel && props.withIcons && item.icon"
              itemprop="name"
              :content="item.label"
            >
            <span
              v-else
              itemprop="name"
            >
              <slot
                name="breadcrumb-item-label"
                :item="item"
                :index="key"
              >
                {{ item.label }}
              </slot>
            </span>

            <meta
              itemprop="position"
              :content="`${key + 1}`"
            >
          </span>
        </slot>
      </li>
    </ul>
  </nav>
</template>

<style lang="scss">
.breadcrumb {
  &.is-narrow {
    margin-bottom: 10px;
  }

  ul {
    li {
      &:first-child {
        .breadcrumb-item {
          padding-inline-start: 0;
        }
      }

      .breadcrumb-item {
        font-family: var(--font);
        color: var(--light-text);
        padding: 0 0.75em;

        .icon {
          &.is-solo {
            .iconify {
              top: 2px;
            }
          }

          .iconify {
            position: relative;
            top: 0;
            font-size: 16px;
            height: 16px;
            min-width: 16px;
          }
        }
      }

      a {
        &.breadcrumb-item {
          &:hover {
            color: var(--primary);
          }
        }
      }
    }
  }
}

.is-dark {
  .breadcrumb {
    ul {
      li {
        a {
          &.breadcrumb-item {
            &:hover {
              color: var(--primary);
            }
          }
        }
      }
    }
  }
}
</style>
