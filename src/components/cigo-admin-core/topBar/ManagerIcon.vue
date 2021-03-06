<template>
<div class="cigo-manager-icon">
    <div class="line"></div>
    <cigo-icon-font v-if="!userInfo.img_info || !userInfo.img_info.signed_url" class="icon avatar" @click.stop="profile" :name="'cigoadmin-icon-touxiangtongyong'" title="管理员"></cigo-icon-font>
    <img v-if="userInfo.img_info && userInfo.img_info.signed_url" class="icon avatar" :src="userInfo.img_info.signed_url" @click.stop="profile" title="管理员" />
    <span class="user-name" @click.stop="profile">{{userInfo.nickname || userInfo.username}}</span>
    <div class="menu-list-container" @mouseenter="hoverMoreIcon(true)" @mouseleave="hoverMoreIcon(false)">
        <cigo-icon-font class="icon more" :name="'cigoadmin-icon-gengduo'" title="更多"></cigo-icon-font>
        <div class="menu-list">
            <span class="menu-item" @click="profile">个人信息</span>
            <span class="divider"></span>
            <span class="menu-item" @click="jumpTo('/frame/manager')">管理员</span>
            <span class="menu-item" @click="jumpTo('/frame/authGroup')">角色管理</span>
            <span class="menu-item" @click="jumpTo('/settings')">系统设置</span>
            <span class="divider"></span>
            <span class="menu-item" @click="logout">退出登录</span>
        </div>
    </div>
</div>
</template>

<script lang="ts">
import {
    defineComponent,
    toRefs
} from "vue";
import CigoIconFont from "@/components/cigo-ui/unit/basic/cigo-icon-font.vue";

import {
    systemStore
} from "@/store/index";
import router from "@/router/index";
import {
    TweenMax
} from "gsap";
import Profile from "@/components/cigo-admin-core/topBar/Profile.vue";
import cigoLayer from "@/components/cigo-layer";

export default defineComponent({
    name: "CigoManagerIcon",
    components: {
        CigoIconFont
    },
    setup() {
        const profile = () => {
            cigoLayer.component({
                component: Profile,
                width: "800px",
                height: "600px",
                maskClose: false,
                layerData: {
                    title: "个人信息"
                }
            });
        };
        const jumpTo = (path: string) => {
            console.log("跳转:", path);
            router.push(path);
        };
        const hoverMoreIcon = (inOutFlag: boolean) => {
            TweenMax.to(
                ".cigo-manager-icon>.menu-list-container>.menu-list",
                0.5, {
                    width: inOutFlag ? "130px" : "0px",
                    height: inOutFlag ? "190px" : "0px",
                    opacity: inOutFlag ? 1 : 0,
                    delay: inOutFlag ? 0 : 0.1
                }
            );
            TweenMax.to(
                ".cigo-manager-icon>.menu-list-container>.menu-list>.menu-item",
                0.5, {
                    width: inOutFlag ? "130px" : "0px",
                    height: inOutFlag ? "38px" : "0px",
                    opacity: inOutFlag ? 1 : 0,
                    delay: inOutFlag ? 0.1 : 0
                }
            );
        };
        const logout = () => {
            router.push("/frame/logoutTip");
        };

        return {
            ...toRefs(systemStore.getState()),
            profile,
            jumpTo,
            hoverMoreIcon,
            logout
        };
    }
});
</script>

<style lang="scss">
.cigo-manager-icon {
    display: flex;
    flex-direction: row;
    align-items: center;
    position: relative;

    .line {
        background-color: #ccc;
        width: 1px;
        height: 15px;
        margin-right: 10px;
    }

    .icon {
        color: #666;
        cursor: pointer !important;
    }

    .icon:hover {
        background-color: #cccccc55;
    }

    .user-name {
        font-size: 14px;
        color: #222;
        margin-right: 5px;
        cursor: pointer;
    }

    .avatar {
        width: 35px;
        height: 35px;
        padding: 5px;
        border-radius: 50%;
        margin-right: 5px;
    }

    .menu-list-container {
        display: flex;
        position: relative;

        .more {
            width: 25px;
            height: 35px;
            border-radius: 5px;
        }

        .menu-list {
            z-index: 200;
            position: absolute;
            opacity: 1 !important;
            display: flex;
            flex-direction: column;
            top: 35px;
            right: 0px;
            width: 0px;
            height: 0px;
            background-color: #fff;
            border: 1px solid #f0f0f0;
            overflow: hidden;
            box-shadow: 2px 2px 10px #e5e5e5;

            .divider {
                height: 1px;
                background-color: #f0f0f0;
            }

            .menu-item {
                display: flex;
                flex-direction: row;
                justify-content: center;
                align-items: center;
                padding: 5px 10px;
                color: #555;
                font-size: 13px;
                min-width: 130px;
                width: 0px;
                height: 0px;
                white-space: nowrap;
                cursor: pointer;
            }

            .menu-item:hover {
                background-color: #f0f0f0;
            }
        }
    }
}
</style>
