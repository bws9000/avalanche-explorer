<template>
    <div id="validators_meta" class="card meta_data">
        <div class="header">
            <h2>
                Validator Stats
                <TooltipHeading
                    content="A validator is a node that is responsible for verifying transactions on a blockchain"
                ></TooltipHeading>
            </h2>
            <v-tabs
                class="tabs"
                @change="typeChange"
                active-class="tab_active"
                height="32"
                hide-slider
            >
                <v-tab>Active</v-tab>
                <v-tab>Pending</v-tab>
            </v-tabs>
        </div>
        <div class="stats">
            <article>
                <img :src="require(`@/assets/ava_price-${imgColor}.png`)" />
                <div class="stat">
                    <p class="label">
                        Total {{toggle}} Stake Amount
                        <TooltipMeta
                            content="Total value of AVAX locked to secure Avalanche"
                        ></TooltipMeta>
                    </p>
                    <p class="meta_val">
                        {{totalStake}}
                        <span class="unit">AVAX</span>
                    </p>
                </div>
            </article>
            <article>
                <img :src="require(`@/assets/validators-${imgColor}.png`)" />
                <div class="stat">
                    <p class="label">
                        {{toggle}} Validators
                        <TooltipMeta
                            content="Total number of nodes validating transactions on Avalanche"
                        ></TooltipMeta>
                    </p>
                    <p class="meta_val">{{totalValidatorsCount.toLocaleString()}}</p>
                </div>
            </article>
        </div>
    </div>
</template>

<script lang="ts">
import "reflect-metadata";
import { Vue, Component, Prop } from "vue-property-decorator";
import { AVALANCHE_SUBNET_ID } from "@/store/modules/platform/platform";
import TooltipHeading from "@/components/misc/TooltipHeading.vue";
import TooltipMeta from "@/components/misc/TooltipMeta.vue";
import { DEFAULT_NETWORK_ID } from "@/store/modules/network/network";

@Component({
    components: {
        TooltipHeading,
        TooltipMeta
    }
})
export default class Metadata extends Vue {
    toggle: string = "active"; // active | pending

    typeChange(val: string) {
        this.toggle = val ? "pending" : "active";
        this.$emit("toggle", this.toggle);
    }

    get totalStake() {
        let valBig = this.toggle === "active"
            ? this.$store.getters["Platform/totalStake"]
            : this.$store.getters["Platform/totalPendingStake"];
        return valBig.div(Math.pow(10, 9)).toLocaleString();
    }

    get totalValidatorsCount() {
        return this.toggle === "active"
            ? this.$store.getters["Platform/totalValidators"]
            : this.$store.getters["Platform/totalPendingValidators"];
    }

    get imgColor(): string {
        return (DEFAULT_NETWORK_ID === 1) ? "testnet" : "testnet";
    }
}
</script>

<style scoped lang="scss">

.meta_data {
    margin-bottom: 30px;

    .header {
        display: flex;
        justify-content: space-between;
    }
}

.stats {
    display: grid;
    width: 100%;
    grid-template-columns: 1fr 1fr max-content;

    > article {
        padding: 30px 15px 0;
        text-align: left;
        line-height: 1.4em;
        display: flex;
        flex-direction: row;
        align-items: center;
    }

    img {
        object-fit: contain;
        width: 40px;
        margin-right: 20px;
    }

    .stat {
        display: flex;
        flex-direction: column;

        p {
            font-weight: 400; /* 700 */
        }

        .label {
            text-transform: capitalize;
            color: $primary-color;
            font-size: 16px;
            font-weight: 700;
            margin-bottom: 6px;
        }

        .meta_val {
            font-size: 32px;
            line-height: 1em;

            .unit {
                font-size: 14px;
                opacity: 0.7;
            }
        }
    }
}

@include smOnly {
    .stats {
        grid-template-columns: 50% 50%;
        grid-template-rows: max-content;

        > div {
            padding: 30px 0 0;
        }

        img {
            width: 24px;
        }

        .stat {
            .label {
                font-size: 13px;
            }

            .meta_val {
                font-size: 20px;

                .unit {
                    font-size: 14px;
                }
            }
        }
    }
}

@include xsOnly {
    .meta_data {
        margin-bottom: 10px;
    }

    .stats {
        grid-template-columns: none;

        > article {
            padding: 15px 0 0;
        }

        img {
            display: none;
        }
    }
}

.tabs {
    flex-direction: row-reverse;
    display: inline-block;
    width: max-content;
    flex-grow: 0;
}

.v-tab {    
    color: $primary-color !important;
    background-color: transparent;
    font-size: 13px;
    font-weight: 400;
    letter-spacing: 0;
    margin: 0;
    text-transform: none;
}

.tab_active {
    border-bottom: 4px solid $secondary-color;
}

@include smOnly {
    .header {
        flex-direction: column;
    }

    .tabs {
        margin-top: 20px;
        width: 100%;
    }

    .v-tab {
        flex-grow: 1;
    }

    .meta_data {
        grid-template-columns: none;
        grid-template-rows: max-content max-content max-content;

        > div {
            text-align: left;
            padding: 0;
        }
    }
}
</style>
<style lang="scss">


#validators_meta {
    
    .v-tab.v-tab {
        font-family: "Rubik", sans-serif;
        text-transform: uppercase;
        font-weight: 500;
    }

    .v-application .primary--text {
        color: $primary-color !important;
        caret-color: $primary-color !important;
    }

    .v-tabs-slider-wrapper {
        color: $secondary-color;
        caret-color: $secondary-color;
    }
}

</style>