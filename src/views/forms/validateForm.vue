<template>
    <div class="contentDiv">
        <div class="posFix">
            <el-card class="box-card">
                <el-form ref="queryForm" :model="queryFormData" label-width="100px">
                    <el-row>
                        <el-col :span="7"><div class="grid-content">
                            <el-form-item label="商场名称：">
                                <el-select class="w100" v-model="queryFormData.mallCode" placeholder="请选择商场" filterable clearable>
                                    <el-option
                                        v-for="(item, index) in mallsListOptions"
                                        :key="index"
                                        :label="item.mallFullName"
                                        :value="item.mallCode">
                                    </el-option>
                                </el-select>
                            </el-form-item>
                        </div></el-col>
                        <el-col :span="7"><div class="grid-content">
                            <el-form-item label="电子券名称：">
                                <el-autocomplete
                                    class="w100"
                                    v-model="queryFormData.couponName"
                                    :fetch-suggestions="queryCouponNamesList"
                                    placeholder="请输入电子券名称"
                                    :trigger-on-focus="false">
                                </el-autocomplete>
                            </el-form-item>
                        </div></el-col>
                        <el-col :span="7"><div class="grid-content">
                            <el-form-item label="电子券种类：">
                                <el-select v-model="queryFormData.couponKind" placeholder="请选择电子券种类" clearable>
                                    <el-option
                                        label="全部"
                                        :value="null">
                                    </el-option>
                                    <el-option
                                        v-for="(item, index) in couponClassOptions"
                                        :key="index"
                                        :label="item.label"
                                        :value="item.value">
                                    </el-option>
                                </el-select>
                            </el-form-item>
                        </div></el-col>
                        <el-col :span="3" class="search-show-more">
                            <div @click="showMore = !showMore">
                                <span>展开更多</span>
                                <i class="icon-show-more el-icon-arrow-down" :class="{'arrowRotate':!showMore}"></i>
                            </div>
                        </el-col>
                    </el-row>
                    <el-collapse-transition>
                        <div v-show="showMore">
                            <el-row>
                                <el-col :span="7"><div class="grid-content">
                                    <el-form-item label="电子券类型：">
                                        <el-select v-model="queryFormData.couponType" placeholder="请选择电子券类型" clearable>
                                            <el-option
                                                label="全部"
                                                :value="null">
                                            </el-option>
                                            <el-option
                                                v-for="(item, index) in couponTypeOptions"
                                                :key="index"
                                                :label="item.label"
                                                :value="item.value">
                                            </el-option>
                                        </el-select>
                                    </el-form-item>
                                </div></el-col>
                                <!-- <el-col :span="7"><div class="grid-content">
                                    <el-form-item label="生效状态：">
                                        <el-select v-model="queryFormData.name" placeholder="请选择生效状态">
                                            <el-option
                                                v-for="(item, index) in effectiveStateOptions"
                                                :key="index"
                                                :label="item.label"
                                                :value="item.value">
                                            </el-option>
                                        </el-select>
                                    </el-form-item>
                                </div></el-col> -->
                                <el-col :span="7"><div class="grid-content">
                                    <el-form-item label="电子券状态：">
                                        <el-select v-model="queryFormData.isVaildFlag" placeholder="请选择电子券状态">
                                            <el-option
                                                v-for="(item, index) in isVaildFlagOptions"
                                                :key="index"
                                                :label="item.label"
                                                :value="item.value">
                                            </el-option>
                                        </el-select>
                                    </el-form-item>
                                </div></el-col>
                                <el-col :span="7"><div class="grid-content">
                                    <el-form-item label="审核状态：">
                                        <el-select v-model="queryFormData.auditStatus" placeholder="请选择审核状态" clearable>
                                            <el-option
                                                label="全部"
                                                :value="null">
                                            </el-option>
                                            <template v-for="(item, index) in examineStateOptions">
                                                <!-- <el-option
                                                    v-if='item.value == 1 || item.value == 4'
                                                    :key="index"
                                                    :label="item.label"
                                                    :value="item.value">
                                                </el-option> -->
                                                <el-option
                                                    :key="index"
                                                    :label="item.label"
                                                    :value="item.value">
                                                </el-option>
                                            </template>
                                        </el-select>
                                    </el-form-item>
                                </div></el-col>
                            </el-row>
                            <!-- <el-row>
                                <el-col :span="10"><div class="grid-content">
                                    <el-form-item label="兑换时间：">
                                        <el-date-picker
                                            class="w100"
                                            v-model="queryFormData.exchangeTime"
                                            type="datetimerange"
                                            range-separator="至"
                                            start-placeholder="开始时间"
                                            end-placeholder="结束时间"
                                            unlink-panels
                                            value-format="yyyy-MM-dd HH:mm:ss"
                                            :default-time="['00:00:00', '23:59:59']"
                                            clearable>
                                        </el-date-picker>
                                    </el-form-item>
                                </div></el-col>
                                <el-col :span="10"><div class="grid-content">
                                    <el-form-item label="核销时间：">
                                        <el-date-picker
                                            class="w100"
                                            v-model="queryFormData.chargedOffTime"
                                            type="datetimerange"
                                            range-separator="至"
                                            start-placeholder="开始时间"
                                            end-placeholder="结束时间"
                                            unlink-panels
                                            value-format="yyyy-MM-dd HH:mm:ss"
                                            :default-time="['00:00:00', '23:59:59']"
                                            clearable>
                                        </el-date-picker>
                                    </el-form-item>
                                </div></el-col>
                            </el-row> -->
                        </div>
                    </el-collapse-transition>
                </el-form>
                <div class="formBtnDiv">
                    <el-button type="primary" :loading="this.queryBtnLoadFlag" @click="queryDataBtn">查询</el-button>
                    <el-button @click="clearQueryData">清空</el-button>
                </div>
            </el-card>
            <el-card class="box-card">
                <div class="tabBtnDiv">
                    <el-button class="tabBtnSty main_button"  @click="couponManageBtn('Create')">新建</el-button>
                    <el-button class="tabBtnSty" @click="couponManageBtn('Edit')">修改</el-button>
                    <el-button class="tabBtnSty"   :loading="delBtnLoadFlag" @click="couponManageBtn('Del')">删除</el-button>
                    <el-button class="tabBtnSty" @click="couponManageBtn('Info')">查看</el-button>
                    <el-button class="tabBtnSty" :loading="batchBtnLoadFlag" @click="couponManageBtn('Batch')">电子券批次</el-button>
                    <el-button class="tabBtnSty"  @click="couponManageBtn('CreateMS')">新建福袋</el-button>
                    <el-popover
                        placement="bottom-start"
                        width="180"
                        trigger="click">
                        <ul class="max-h-350">
                            <li v-for="(item, index) in tabListShowControl" :key="index">
                                <el-checkbox v-model="item.showFlag" @change="updateSessionShowFlag">{{item.showName}}</el-checkbox>
                            </li>
                        </ul>
                        <el-button class="tabControlBtn" icon="el-icon-menu" slot="reference"></el-button>
                    </el-popover>
                </div>
                <div class="tabDiv mg-b-24">
                    <el-table
                        border
                        maxHeight="500"
                        :data="tableDataList"
                        highlight-current-row
                        @row-click="handleCurrentChange"
                        style="width: 100%"
                        row-key="id"
                        :tree-props="{children: 'subVoList'}">
                        <el-table-column
                            label="单选"
                            width="80"
                            fixed>
                            <template slot-scope="scope">
                                <div class="radioTabDiv">
                                    <div v-if="scope.row.mainCouponFlag !== false">
                                        <el-radio :label="scope.row.id" v-model="currentTabIdData">&nbsp;</el-radio>
                                    </div>
                                    <div class="btnMask"></div>
                                </div>
                            </template>
                        </el-table-column>
                        <el-table-column
                            v-if="tabListShowControl[0].showFlag"
                            label="电子券种类"
                            width="150"
                            align="center">
                            <template slot-scope="scope">
                                <span>{{scope.row.couponKindDesc}}</span>
                                <span v-show="scope.row.mainCouponFlag === true">（主）</span>
                                <span v-show="scope.row.mainCouponFlag === false">（子）</span>
                            </template>
                        </el-table-column>
                        <el-table-column
                            v-if="tabListShowControl[1].showFlag"
                            prop="couponTypeDesc"
                            label="电子券类型"
                            width="150"
                            align="center">
                        </el-table-column>
                        <el-table-column
                            v-if="tabListShowControl[2].showFlag"
                            label="商场"
                            width="220"
                            align="center">
                            <template slot-scope="scope">
                                <ul>
                                    <li v-for="(item, index) in scope.row.mallVoList" :key="index">
                                        <span>{{item.mallName}}</span>
                                    </li>
                                </ul>
                            </template>
                        </el-table-column>
                        <el-table-column
                            v-if="tabListShowControl[3].showFlag"
                            prop="id"
                            label="电子券ID"
                            width="180"
                            align="center">
                        </el-table-column>
                        <el-table-column
                            v-if="tabListShowControl[4].showFlag"
                            prop="couponName"
                            label="电子券名称"
                            width="180"
                            align="center">
                        </el-table-column>
                        <el-table-column
                            v-if="tabListShowControl[5].showFlag"
                            prop="auditStatusDesc"
                            label="审核状态"
                            width="180"
                            align="center">
                        </el-table-column>
                         <el-table-column label="操作" align="center" width='120' fixed="right">
                            <template slot-scope="scope">
                                <el-button type='text' v-if="scope.row.couponGrantWay==3&&scope.row.mainCouponFlag !== false" @click.stop='previewDetail(scope.row)'>预览</el-button>
                                <el-button type='text'  v-if="scope.row.couponGrantWay==3&&scope.row.mainCouponFlag !== false" @click.stop='produceCode(scope.row)'>生成小程序码</el-button>
                                <el-button type='text' v-if="scope.row.couponGrantWay!=3&&scope.row.mainCouponFlag !== false" class="diabledBtn" disabled>预览</el-button>
                                <el-button type='text'  v-if="scope.row.couponGrantWay!=3&&scope.row.mainCouponFlag !== false" class="diabledBtn" disabled>生成小程序码</el-button>
                            </template>
                        </el-table-column>
                    </el-table>
                </div>
                <div class="pageDiv align_right">
                    <el-pagination
                        @size-change="pageSizeChange"
                        @current-change="pageCurrentChange"
                        :current-page="pageNum"
                        :page-sizes="[20, 50, 100, 200, 300, 400]"
                        :page-size="pageSize"
                        layout="total, sizes, prev, pager, next, jumper"
                        :total="pageTotal">
                    </el-pagination>
                </div>
            </el-card>
            <el-dialog
                :title="ceDialogFormData.ceDialogTitle"
                :visible.sync="ceDialogVisible"
                width="50%"
                @closed="closeCEDialogVisible"
                :close-on-click-modal="false">
                <el-form class="max-h-500" ref="ceDialogForm" :model="ceDialogFormData" label-width="115px">
                    <el-form-item class="mg-b-0" label="是否集团券：">
                        <el-radio-group v-model="ceDialogFormData.groupFlag">
                            <el-radio :label="true">是</el-radio>
                            <el-radio :label="false">否</el-radio>
                        </el-radio-group>
                    </el-form-item>
                    <el-form-item class="mg-b-0" label="适用商场清单：" v-show="ceDialogFormData.groupFlag">
                        <el-checkbox-group v-model="ceDialogFormData.checkList">
                            <el-checkbox v-for="(item, index) in mallDialogDataList" :key="index" :label="item.mallCode">{{item.mallFullName}}</el-checkbox>
                        </el-checkbox-group>
                    </el-form-item>
                </el-form>
                <span slot="footer" class="dialog-footer">
                    <el-button type="primary" @click="ceDialogBtn">下一步</el-button>
                </span>
            </el-dialog>
            <el-dialog
                title="电子券发放"
                :visible.sync="sendDialogVisible"
                width="50%"
                @closed="closeSendDialogVisible"
                :close-on-click-modal="false">
                <div class="dialogTitleSty mg-b-24">
                    <span>电子券ID：{{sendDialogFormData.eCouponId}}</span>
                    <span>电子券名称：{{sendDialogFormData.eCouponName}}</span>
                </div>
                <el-form class="max-h-500" ref="sendDialogForm" :model="sendDialogFormData" label-width="120px" :rules="sendDialogFormRules">
                    <el-form-item class="tipPosR" label="当前发券批次：" prop="batchId">
                        <el-select v-model="sendDialogFormData.batchId" placeholder="请选择电子券批次" @change="changeBatchIdSelect" clearable>
                            <el-option
                                v-for="(item, index) in couponBatchOptions"
                                :key="index"
                                :label="item.chargedOffBeginTime&&item.chargedOffEndTime? item.batchName+' '+item.chargedOffBeginTime +'~' +item.chargedOffEndTime :item.batchName"
                                :value="item.id">
                            </el-option>
                        </el-select>
                        <div class="tipTextPosA" v-show="sendDialogFormData.sendMaxTotalShowFlag">
                            <span>最大可发券数量：</span>
                            <span class="textRedColor">{{sendDialogFormData.couponTotal}}</span>
                        </div>
                    </el-form-item>
                    <el-form-item class="formCheckSty" label="会员手机：" prop="mobile">
                        <el-input v-model="sendDialogFormData.mobile" placeholder="会员手机/会员号二选一" @blur="sendDialogBlurInput('mobile')" @keyup.enter.native="sendDialogBlurInput('mobile')"></el-input>
                    </el-form-item>
                    <el-form-item label="会员号：" prop="memberCode">
                        <el-input v-model="sendDialogFormData.memberCode" placeholder="会员手机/会员号二选一" @blur="sendDialogBlurInput('memberCode')" @keyup.enter.native="sendDialogBlurInput('memberCode')"></el-input>
                    </el-form-item>
                    <el-form-item class="tipPosR" label="会员名：">
                        <el-input v-model="sendDialogFormData.memberName" disabled></el-input>
                        <div class="tipTextPosA" v-show="sendDialogFormData.memberFoundShowFlag">
                            <span class="textRedColor">未查询到会员信息！</span>
                        </div>
                    </el-form-item>
                    <el-form-item class="formCheckSty tipPosR" label="发券数量：" prop="grantCouponNum">
                        <el-input v-model="sendDialogFormData.grantCouponNum" placeholder="请输入发券数量"></el-input>
                        <div class="tipTextPosA" v-show="sendDialogFormData.memberTotalShowFlag">
                            <span>单个会员可发券数量：</span>
                            <span class="textRedColor" v-show="sendDialogFormData.memberTotal !== 0">{{sendDialogFormData.memberTotal}}</span>
                            <span class="textRedColor" v-show="sendDialogFormData.memberTotal === 0">无限制</span>
                        </div>
                    </el-form-item>
                </el-form>
                <span slot="footer" class="dialog-footer">
                    <el-button @click="closeSendDialogVisible">取消</el-button>
                    <el-button type="primary" :loading="sendDialogBtnLoadFlag" @click="sendDialogBtn">确定发送</el-button>
                </span>
            </el-dialog>
            <!-- 预览 -->
            <el-dialog
                :visible.sync="previewDialogVisible"
                width="640px"
               >
                <!-- title="预览" -->
                <div class="goodsDetails">
                    <div class="banner">
                        <img :src="bannerImg" class="banner_img" mode="widthFix" />
                        <div class="banner-details">
                            <p>{{couponDetails.couponName}}</p>
                            <p>{{couponDetails.couponSubTitle ? couponDetails.couponSubTitle : ''}}</p>
                            <div class="banner-details_item">
                                <p class="banner-details_date">
                                    活动时间：
                                    <span :class="{'redColor':couponDetails.exchangeBeginTime==null}">{{couponDetails.exchangeBeginTime||'null'}}-{{couponDetails.exchangeEndTime||'null'}}</span>
                                </p>
                            </div>
                            <p class="banner-details_num">
                                剩余：
                                <span class="number">{{couponDetails.remainAmount}}</span>
                            </p>
                        </div>
                    </div>
                    <div class="hr_class"></div>
                    <div class="buy">
                        <p>购买数量</p>
                        <div class="buyNum">
                            <p class="onlyBuy">{{limitBuy}}</p>
                            <div class="buyNum_item">
                                <div class="buy_btn">-</div>
                                <input type="number" class="buy_input" :value="price"  />
                                <div class="buy_btn" >+</div>
                            </div>
                        </div>
                    </div>
                    <div class="hr_class"></div>
                    <div class="subtitle">
                        <div class="subtitle_item">
                            <p class="name">物品详情</p>
                        </div>
                        <p class="subtitle_text">
                            <span>{{couponDetails.couponDesc}}</span>
                        </p>
                    </div>
                    <div class="hr_class"></div>
                    <div class="subtitle bottomClass">
                        <div class="subtitle_item">
                            <p class="name">使用规则</p>
                        </div>
                        <p class="subtitle_text">
                            <span>{{couponDetails.useRuleDesc}}</span>
                        </p>
                    </div>
                </div>
            </el-dialog>
            <!-- 生成二维码 -->
             <el-dialog
                :visible.sync="produceCodeDialogVisible"
                width="50%"
               >
               <module-title :title="couponName" :collapsible="false"></module-title>
               <div class="codeBox">
                   <!-- <img src="../../../assets/images/add.png" alt=""> -->
                   <img :src="codeImage" alt="">
               </div>
               <div class="uploadBox">
                   <el-button type="primary" @click="clickToUpload">点击下载</el-button>
               </div>
               
            </el-dialog>
        </div>
    </div>
</template>

<script>
// import baseAPI from '@/api/baseAPI.js'
// import API from '@/api/couponManage.js'
import moduleTitle from '../../components/collapse'
// import Utils from '@/utils/utils.js'
import { allDatas } from '../../mixin/data'
export default {
    name:'eCouponManage',
    mixins: [allDatas],
    components:{
       moduleTitle 
    },
    data() {
        // 发券 Dialog 对话框 - 会员手机校验规则
        var validateMobile = (rule, value, callback) => {
            if(!value) {
                if(this.sendDialogFormData.memberCode) {
                    callback();
                } else {
                    return callback(new Error('会员手机/会员号二选一填写即可！'));
                }
            }
            callback();
        };
        // 发券 Dialog 对话框 - 会员号校验规则
        var validateMemberCode = (rule, value, callback) => {
            if(!value) {
                if(this.sendDialogFormData.mobile) {
                    this.$refs['sendDialogForm'].clearValidate(['mobile']);
                    callback();
                } else {
                    return callback(new Error('会员手机/会员号二选一填写即可！'));
                }
            }
            callback();
        };
        // 发券 Dialog 对话框 - 发券数量校验规则
        var validateGrantCouponNum = (rule, value, callback) => {
            let re1 = /(^[0-9]{1,10}$)|(^[0-9]{1,10}[\.]{1}[0-9]{1,2}$)/;
            let re2 = /(^[1-9]{1}$)|(^[0-9]{1}[\.]{1}[0-9]{1}$)|(10)/;
            let re3 = /(^[0-9]{1,10}$)/;
            if(!value) {
                this.sendDialogFormData.memberTotalShowFlag = false;
                return callback(new Error('发券数量不能为空！'));
            }
            if(!re3.test(value)) {
                this.sendDialogFormData.memberTotalShowFlag = false;
                return callback(new Error('发券数量格式错误！'));
            }
            if(Number(value) === 0) {
                this.sendDialogFormData.memberTotalShowFlag = false;
                return callback(new Error('发券数量不能为0！'));
            }
            if(this.sendDialogFormData.memberTotal) {
                if(Number(value) > this.sendDialogFormData.memberTotal && Number(value) !== 0) {
                    this.sendDialogFormData.memberTotalShowFlag = false;
                    return callback(new Error('发券数量不能大于限获数量 ' + this.sendDialogFormData.memberTotal + ' 张！'));
                }
            }
            if(this.sendDialogFormData.couponTotal) {
                if(Number(value) > this.sendDialogFormData.couponTotal) {
                    this.sendDialogFormData.memberTotalShowFlag = false;
                    return callback(new Error('发券数量不能大于最大可发券数量！'));
                }
            }
            this.sendDialogFormData.memberTotalShowFlag = true;
            callback();
        };
        return {
            // 发券 Dialog 对话框表单校验规则
            sendDialogFormRules: {
                batchId: [
                    { required: true, message: '请选择电子券批次', trigger: 'change' }
                ],
                mobile: [
                    { validator: validateMobile, trigger: 'blur' }
                ],
                memberCode: [
                    { validator: validateMemberCode, trigger: 'blur' }
                ],
                grantCouponNum: [
                    { validator: validateGrantCouponNum, trigger: 'blur' }
                ]
            },
            // 查询表单显示控制
            showMore: false,
            // 商场选择器数据
            mallsListOptions: [],
            // 电子券种类选择器数据
            couponClassOptions: [
                // {value: null, label: '全部'},
                // {value: '1', label: '普通券'},
                // {value: '2', label: '主子券（福袋）'}
            ],
            // 电子券类型选择器数据
            couponTypeOptions: [
                {value: null, label: '全部'},
                {value: '1', label: '现金券'},
                {value: '2', label: '满减券'},
                {value: '3', label: '满折券'},
                {value: '4', label: '停车券'},
                {value: '5', label: '积分券'},
                {value: '6', label: '礼品券'}
            ],
            // 生效状态选择器数据
            effectiveStateOptions: [
                {value: null, label: '全部'},
                {value: '0', label: '未生效'},
                {value: '1', label: '已生效'}
            ],
            // 审核状态选择器数据
            examineStateOptions: [
                {value: null, label: '全部'},
                {value: '1', label: '待提交审核'},
                {value: '2', label: '审核拒绝'},
                {value: '3', label: '审核中'},
                {value: '4', label: '审核通过'}
            ],
            // 电子券状态选择器数据
            isVaildFlagOptions: [
                {value: null, label: '全部'},
                {value: '1', label: '有效'},
                {value: '0', label: '无效'}
            ],
            // 查询表单数据
            queryFormData: {
                mallCode: null,
                couponName: null,
                couponKind: null,
                couponType: null,
                auditStatus: null,
                // exchangeTime: [],
                // chargedOffTime: [],
                isVaildFlag: null
            },
            // 查询表单 Temp 数据
            queryFormDataTemp: {},
            // 查询按钮加载标志
            queryBtnLoadFlag: false,
            // 删除按钮加载标志
            delBtnLoadFlag: false,
            // 电子券批次按钮加载标志
            batchBtnLoadFlag: false,
            // 分页器数据 - 当前页数、每页数据条数、数据总条数
            pageNum: 1,
            pageSize: 20,
            pageTotal: null,
            // 表格列显示控制
            tabListShowControl: [
                {value: 'couponKindDesc', showFlag: true, showName: '电子券种类'},
                {value: 'couponType', showFlag: true, showName: '电子券类型'},
                {value: 'mallVoList', showFlag: true, showName: '商场'},
                {value: 'id', showFlag: true, showName: '电子券ID'},
                {value: 'couponName', showFlag: true, showName: '电子券名称'},
                {value: 'auditStatusDesc', showFlag: true, showName: '审核状态'},
                {value: 'couponWorth', showFlag: true, showName: '电子券价值'},
                {value: 'soldAmount', showFlag: true, showName: '已出售数量'},
                {value: 'chargedOffAmount', showFlag: true, showName: '已核销数量'},
                {value: 'totalAmount', showFlag: true, showName: '总数量'},
                {value: 'usableAmount', showFlag: true, showName: '可用数量'},
                {value: 'couponSubTitle', showFlag: false, showName: '电子券副标题'},
                {value: 'couponGrantWayDesc', showFlag: false, showName: '电子券发放方式'},
                {value: 'purchaseWayDesc', showFlag: false, showName: '电子券兑换类型'},
                {value: 'needActivateFlag', showFlag: false, showName: '是否需要激活'},
                {value: 'maxAcquirableAmount', showFlag: false, showName: '限获数量'},
                {value: 'couponDesc', showFlag: false, showName: '物品描述'},
                {value: 'useRuleDesc', showFlag: false, showName: '使用规则'},
                {value: 'discount', showFlag: false, showName: '折扣'},
                {value: 'thresholdPrice', showFlag: false, showName: '门槛金额'},
                {value: 'cappingPrice', showFlag: false, showName: '封顶金额'},
                {value: 'purchaseChannel', showFlag: false, showName: '购买渠道'},
                {value: 'integralShopDisplayFlag', showFlag: false, showName: '积分商城展示'},
                {value: 'couponOverlayRuleDesc', showFlag: false, showName: '叠加规则'},
                {value: 'shopDedicatedFlag', showFlag: true, showName: '店铺限制'},
                {value: 'groupFlag', showFlag: true, showName: '集团券'},
                {value: 'createBy', showFlag: true, showName: '创建人'},
                {value: 'createTime', showFlag: true, showName: '创建时间'},
                {value: 'modifyBy', showFlag: true, showName: '最后修改人'},
                {value: 'modifyTime', showFlag: true, showName: '最后修改时间'}
            ],
            // 页面列表数据
            tableDataList: [],
            // 表格选中数据
            multipleSelection: [],
            // 表格选中电子券 ID
            currentTabIdData: null,
            // Dialog 对话框显示控制【新建 & 修改】
            ceDialogVisible: false,
            // Dialog 对话框表单数据【新建 & 修改】
            ceDialogFormData: {
                groupFlag: null,
                checkList: [],
                ceDialogTitle: null
            },
            // Dialog 适用商场清单多选框数据
            mallDialogDataList: [],
            // Dialog 对话框显示控制【发券】
            sendDialogVisible: false,
            // Dialog 对话框表单数据【发券】
            sendDialogFormData: {
                eCouponId: null,
                eCouponName: null,
                batchId: null,
                couponTotal: null,
                mobile: null,
                memberCode: null,
                memberName: null,
                grantCouponNum: null,
                sendMaxTotalShowFlag: false,
                memberFoundFlag: false,
                memberFoundShowFlag: false,
                memberTotal: null,
                memberTotalShowFlag: false
            },
            // Dialog 对话框表单数据【发券】 - 电子券批次选择器数据
            couponBatchOptions: [],
            // 确定发送按钮加载标志
            sendDialogBtnLoadFlag: false,
            isSaveAndCreateFlag:false,//如果是保存并新建跳过来的
            previewDialogVisible:false,//预览对话框
            produceCodeDialogVisible:false,//二维码对话框
            couponName:'',
            codeImage:'',
            qrcodeURL: '',
            price: 1,
            userInfo: {},
            nowJf: 66,
            showDialog: false,
            title: '',
            bodyText: '',
            couponDetails: {},
            availableIntegral: 0,
            mobile: null,
            mallCode: '',
            gradeCode: '',
            memberCode: '',
            memberName: '',
            isLoading: false,
            bannerImg: '',
            webViewurl: '',
            limitBuy: ''
        }
    },
    created() {
        console.log('生命周期：created~',this.allMalls);
        // 获取按钮权限
        if(JSON.parse(sessionStorage.getItem('eCouponTabShowFlag'))) {
            let eCouponTabShowTemp = JSON.parse(sessionStorage.getItem('eCouponTabShowFlag'));
            this.tabListShowControl = eCouponTabShowTemp;
        } else {
        
        }
        this.queryFormDataTemp = JSON.parse(JSON.stringify(this.queryFormData));
        this.queryMallsList();
        Promise.all([this.queryTabData()]).then((res) => {
            console.log('res',res);
            if(this.isSaveAndCreateFlag&&res){
                console.log('Batch');
                this.couponManageBtn('Batch')
            } 
            
        }).catch(err => {
            console.log(err)
        })
            
            
           
            
        
    },
    methods: {
    //    下载到本地
        clickToUpload(){
            const image = new Image();
              // 解决跨域 canvas 污染问题
              image.setAttribute("crossOrigin", "anonymous");
              image.onload = function() {
                const canvas = document.createElement("canvas");
                canvas.width = image.width;
                canvas.height = image.height;
                const context = canvas.getContext("2d");
                context.drawImage(image, 0, 0, image.width, image.height);
                //得到图片的base64编码数据
                const url = canvas.toDataURL("image/png");
                // 生成一个 a 标签
                const a = document.createElement("a");
                // 创建一个点击事件
                const event = new MouseEvent("click");
                // 将 a 的 download 属性设置为我们想要下载的图片的名称
                a.download = '';
                // 将生成的 URL 设置为 a.href 属性
                a.href = url;
                // 触发 a 的点击事件
                a.dispatchEvent(event);
                // return a;
              };
              console.log(this.codeImage);
              
              image.src = this.codeImage
            //   image.src = 'http://img1.dev.rs.com/g1/M00/04/3B/wKh6y12qsgGAdh5HAACj9vwYntw206.jpg!'
        },
        // 表格列显示控制
        updateSessionShowFlag() {
            sessionStorage.setItem('eCouponTabShowFlag', JSON.stringify(this.tabListShowControl));
        },
        // 查询商场选择器列表-interface
        queryMallsList() {
            console.log(this.allMalls);
            this.mallsListOptions = this.allMalls;
        },
        // 查询卡券名称输入框列表
        queryCouponNamesList(queryString, cb) {
            if(queryString) {
                let filterData = [];
                API.getByCouponName(queryString).then((res)=>{
                    console.log(res);
                    let returnDatas = res.data.data;
                    for(let i = 0; i < returnDatas.length; i++) {
                        filterData.push({value: returnDatas[i].couponName, label: returnDatas[i].couponName});
                    }
                    cb(filterData);
                }).catch(()=>{
                    cb(filterData);
                });
            }
        },
        // 获取表格数据
       queryTabData() {
            for (let i in this.queryFormDataTemp) {
                if(!this.queryFormDataTemp[i]) {
                    this.queryFormDataTemp[i] = '';
                }
            }
            let pageNum = this.pageNum;
            let pageSize = this.pageSize;
            let couponName = this.queryFormDataTemp.couponName.replace(/(^\s*)|(\s*$)/g, "");
            let couponKind = this.queryFormDataTemp.couponKind;
            let auditStatus = this.queryFormDataTemp.auditStatus;
            let couponType = this.queryFormDataTemp.couponType;
            let mallCodes = this.queryFormDataTemp.mallCode;
            let isVaildFlag = this.queryFormDataTemp.isVaildFlag;
            console.log(this.couponDataLists);
            this.tableDataList = this.couponDataLists.list?this.couponDataLists.list:[];
            this.pageTotal = this.couponDataLists.total?this.couponDataLists.total:0;
            this.queryBtnLoadFlag = false;
            return true
        },
        // 查询按钮事件
        queryDataBtn() {
            this.queryBtnLoadFlag = true;
            this.pageNum = 1;
            this.queryFormDataTemp = JSON.parse(JSON.stringify(this.queryFormData));
            this.queryTabData();
            this.currentTabIdData = null;
            this.$set(this, 'multipleSelection', []);
        },
        // 清空按钮事件
        clearQueryData() {
            this.queryFormData = {
                mallCode: null,
                couponName: null,
                couponKind: null,
                couponType: null,
                auditStatus: null,
                // exchangeTime: [],
                // chargedOffTime: [],
                isVaildFlag: null
            }
        },
        // 分页器 - 当前页改变时触发
        pageCurrentChange(val) {
            this.pageNum = val;
            this.queryTabData();
            this.$set(this, 'multipleSelection', []);
        },
        // 分页器 - 每页条数改变时触发
        pageSizeChange(val) {
            this.pageNum = 1;
            this.pageSize = val;
            this.queryTabData();
            this.$set(this, 'multipleSelection', []);
        },
        // 表格选中事件
        handleCurrentChange(val) {
            console.log(val);
            if(val.id === this.currentTabIdData) {
                this.currentTabIdData = null;
                this.$set(this, 'multipleSelection', []);
            } else {
                this.currentTabIdData = val.id;
                this.$set(this, 'multipleSelection', []);
                this.multipleSelection.push(val);
            }
        },
        // 按钮事件
        couponManageBtn(flag) {
            switch(flag) {
                case 'Create':
                    // this.$openTabSwitch('电子券新建', '/eCouponCreate?webStateFlag=Create');
                    if(this.mallDialogDataList.length > 1) {
                        this.ceDialogFormData.ceDialogTitle = '电子券新建';
                        this.ceDialogVisible = true;
                    } else {
                        this.$openTabSwitch('电子券新建', '/eCouponCreate?webStateFlag=Create&groupFlag=false');
                    }
                    break;
                case 'BatchCreate':
                    this.$openTabSwitch('电子券批量新建', '/eCouponBatchCreate');
                    break;
                case 'Edit':
                    // this.$openTabSwitch('修改电子券', '/eCouponEdit?webStateFlag=Edit');
                    // this.ceDialogFormData.ceDialogTitle = '修改电子券';
                    // this.ceDialogVisible = true;
                    if(this.multipleSelection.length === 0 || this.multipleSelection[0].mainCouponFlag === false) {
                        this.$message({
                            message: '请选择需要修改的电子券！',
                            type: 'error'
                        });
                        return false;
                    } else if(this.multipleSelection.length > 1) {
                        this.$message({
                            message: '系统目前仅支持单张电子券修改！',
                            type: 'error'
                        });
                        return false;
                    } else if(this.multipleSelection[0].auditStatus !== 1 && this.multipleSelection[0].auditStatus !== 2) {
                        this.$message({
                            message: '只有审核状态为待提交审核或审核拒绝的电子券可以修改！',
                            type: 'error'
                        });
                        return false;
                    } else {
                        if(!this.multipleSelection[0].mainCouponFlag) {
                            this.$openTabSwitch('电子券修改', '/eCouponEdit?webStateFlag=Edit&eCouponId=' + this.multipleSelection[0].id);
                        } else {
                            this.$openTabSwitch('电子券福袋修改', '/eCouponEditMS?webStateFlag=Edit&eCouponId=' + this.multipleSelection[0].id);
                        }
                    }
                    break;
                case 'Del':
                    if(this.multipleSelection.length === 0 || this.multipleSelection[0].mainCouponFlag === false) {
                        this.$message({
                            message: '请选择需要删除的电子券！',
                            type: 'error'
                        });
                        return false;
                    } else if(this.multipleSelection.length > 1) {
                        this.$message({
                            message: '系统目前仅支持单张电子券删除！',
                            type: 'error'
                        });
                        return false;
                    } else if(this.multipleSelection[0].auditStatus !== 1 && this.multipleSelection[0].auditStatus !== 2) {
                        this.$message({
                            message: '只有审核状态为待提交审核或审核拒绝的电子券可以删除！',
                            type: 'error'
                        });
                        return false;
                    } else {
                        this.$confirm('请确认是否删除电子券？', '提示', {
                            confirmButtonText: '确定',
                            cancelButtonText: '取消',
                            type: 'warning'
                        }).then(() => {
                            this.delBtnLoadFlag = true;
                            API.delCouponData(this.multipleSelection[0].id).then((res)=>{
                                if(res.data.code === 200) {
                                    this.delBtnLoadFlag = false;
                                    this.$message({
                                        message: '删除成功！',
                                        type: 'success'
                                    });
                                    this.queryTabData();
                                }
                            }).catch(()=>{
                                this.delBtnLoadFlag = false;
                                // this.$message({
                                //     message: '删除失败！',
                                //     type: 'error'
                                // });
                            });
                        }).catch(() => {
                            return false;
                        });
                    }
                    break;
                case 'Info':
                    if(this.multipleSelection.length === 0 || this.multipleSelection[0].mainCouponFlag === false) {
                        this.$message({
                            message: '请选择需要查看的电子券！',
                            type: 'error'
                        });
                        return false;
                    } else if(this.multipleSelection.length > 1) {
                        this.$message({
                            message: '系统目前仅支持单张电子券查看！',
                            type: 'error'
                        });
                        return false;
                    } else {
                        this.$openTabSwitch('电子券查看', '/eCouponInfo?webStateFlag=Info&eCouponId=' + this.multipleSelection[0].id);
                    }
                    break;
                case 'SubmitExamine':
                    if(this.multipleSelection.length === 0 || this.multipleSelection[0].mainCouponFlag === false) {
                        this.$message({
                            message: '请选择需要提交审核的电子券！',
                            type: 'error'
                        });
                        return false;
                    } else if(this.multipleSelection.length > 1) {
                        this.$message({
                            message: '系统目前仅支持单张电子券提交审核！',
                            type: 'error'
                        });
                        return false;
                    } else if(this.multipleSelection[0].auditStatus !== 1 && this.multipleSelection[0].auditStatus !== 2) {
                        this.$message({
                            message: '只有审核状态为待提交审核或审核拒绝的电子券可以提交审核！',
                            type: 'error'
                        });
                        return false;
                    } else {
                        this.$openTabSwitch('电子券提交审核', '/eCouponInfo?webStateFlag=SubmitExamine&eCouponId=' + this.multipleSelection[0].id);
                    }
                    break;
                case 'Examine':
                    if(this.multipleSelection.length === 0 || this.multipleSelection[0].mainCouponFlag === false) {
                        this.$message({
                            message: '请选择需要审核的电子券！',
                            type: 'error'
                        });
                        return false;
                    } else if(this.multipleSelection.length > 1) {
                        this.$message({
                            message: '系统目前仅支持单张电子券审核！',
                            type: 'error'
                        });
                        return false;
                    } else if(this.multipleSelection[0].auditStatus !== 3) {
                        this.$message({
                            message: '只有审核状态为审核中的电子券可以进行审核！',
                            type: 'error'
                        });
                        return false;
                    } else {
                        this.$openTabSwitch('电子券审核', '/eCouponInfo?webStateFlag=Examine&eCouponId=' + this.multipleSelection[0].id);
                    }
                    break;
                case 'Batch':
                    // console.log('跳转开始');
                    
                    // if(!this.isSaveAndCreateFlag){
                        if(this.multipleSelection.length === 0 || this.multipleSelection[0].mainCouponFlag === false) {
                            this.$message({
                                message: '请选择需要批次创建的电子券！',
                                type: 'error'
                            });
                            return false;
                        } else if(this.multipleSelection.length > 1) {
                            this.$message({
                                message: '系统目前仅支持单张电子券批次创建！',
                                type: 'error'
                            });
                            return false;
                        } /* else if(this.multipleSelection[0].auditStatus !== 4) {
                            this.$message({
                                message: '只有审核状态为审核通过的电子券可以批次创建！',
                                type: 'error'
                            });
                            return false;
                        } */ 
                       
                        else {
                            this.batchBtnLoadFlag = true;
                            API.checkAddBefore(this.multipleSelection[0].id).then((res)=>{
                                this.batchBtnLoadFlag = false;
                                if(res.data.code === 200) {
                                    this.$openTabSwitch('电子券批次创建', '/eCouponBatch?eCouponId=' + this.multipleSelection[0].id + '&eCouponName=' + this.multipleSelection[0].couponName+ '&auditStatus=' + this.multipleSelection[0].auditStatus+'&mainCouponFlag='+ this.multipleSelection[0].mainCouponFlag);
                                }
                            }).catch(()=>{
                                this.batchBtnLoadFlag = false;
                            });
                        }
                    // }
                    //  else if(this.isSaveAndCreateFlag){
                    //     // this.batchBtnLoadFlag = true;
                    //         console.log('hahha',this.tableDataList[0]);
                    //             API.checkAddBefore(this.tableDataList[0].id).then((res)=>{
                    //             this.batchBtnLoadFlag = false;
                    //             if(res.data.code === 200) {
                    //                 this.$openTabSwitch('电子券批次创建', '/eCouponBatch?eCouponId=' + this.tableDataList[0].id + '&eCouponName=' + this.tableDataList[0].couponName+ '&auditStatus=' + this.tableDataList[0].auditStatus+'&mainCouponFlag='+ this.tableDataList[0].mainCouponFlag);
                    //             }
                    //         }).catch(()=>{
                    //             this.batchBtnLoadFlag = false;
                    //         });
                    //     }
                    break;
                case 'Send':
                    if(this.multipleSelection.length === 0 || this.multipleSelection[0].mainCouponFlag === false) {
                        this.$message({
                            message: '请选择需要发放的电子券！',
                            type: 'error'
                        });
                        return false;
                    } else if(this.multipleSelection.length > 1) {
                        this.$message({
                            message: '系统目前仅支持单张电子券发放！',
                            type: 'error'
                        });
                        return false;
                    } else if(this.multipleSelection[0].couponGrantWay !== 4) {
                        this.$message({
                            message: '只有获取方式为单独推送的电子券可以发放！',
                            type: 'error'
                        });
                        return false;
                    } else if(this.multipleSelection[0].auditStatus !== 4) {
                        this.$message({
                            message: '只有审核状态为审核通过的电子券可以发放！',
                            type: 'error'
                        });
                        return false;
                    } else {
                        this.sendDialogFormData.eCouponId = this.multipleSelection[0].id;
                        this.sendDialogFormData.eCouponName = this.multipleSelection[0].couponName;
                        this.queryCouponBatchList(this.sendDialogFormData.eCouponId);
                        this.sendDialogVisible = true;
                    }
                    break;
                case 'BatchSend':
                    if(this.multipleSelection.length === 0 || this.multipleSelection[0].mainCouponFlag === false) {
                        this.$message({
                            message: '请选择需要批量发放的电子券！',
                            type: 'error'
                        });
                        return false;
                    } else if(this.multipleSelection.length > 1) {
                        this.$message({
                            message: '系统目前仅支持单张电子券批量发放！',
                            type: 'error'
                        });
                        return false;
                    } else if(this.multipleSelection[0].couponGrantWay !== 4) {
                        this.$message({
                            message: '只有获取方式为单独推送的电子券可以批量发放！',
                            type: 'error'
                        });
                        return false;
                    } else if(this.multipleSelection[0].auditStatus !== 4) {
                        this.$message({
                            message: '只有审核状态为审核通过的电子券可以批量发放！',
                            type: 'error'
                        });
                        return false;
                    } else {
                        this.$openTabSwitch('电子券批量发券', '/eCouponBatchSend?eCouponId=' + this.multipleSelection[0].id + '&eCouponName=' + this.multipleSelection[0].couponName);
                    }
                    break;
                case 'CreateMS':
                    this.$openTabSwitch('电子券福袋新建', '/eCouponCreateMS?webStateFlag=Create&groupFlag=false');
                    break;
                case 'TakeEffectEdit':
                    if(this.multipleSelection.length === 0 || this.multipleSelection[0].mainCouponFlag === false) {
                        this.$message({
                            message: '请选择需要生效后变更的电子券！',
                            type: 'error'
                        });
                        return false;
                    } else if(this.multipleSelection.length > 1) {
                        this.$message({
                            message: '系统目前仅支持单张电子券生效后变更！',
                            type: 'error'
                        });
                        return false;
                    } else if(this.multipleSelection[0].auditStatus !== 4) {
                        this.$message({
                            message: '只有审核状态为审核通过的电子券可以生效后变更！',
                            type: 'error'
                        });
                        return false;
                    } else {
                        if(!this.multipleSelection[0].mainCouponFlag) {
                            this.$openTabSwitch('电子券生效后变更', '/eCouponEdit?webStateFlag=TakeEffectEdit&eCouponId=' + this.multipleSelection[0].id);
                        } else {
                            this.$openTabSwitch('电福袋生效后变更', '/eCouponEditMS?webStateFlag=TakeEffectEdit&eCouponId=' + this.multipleSelection[0].id);
                        }
                    }
                    break;
            }
        },
        // Dialog 对话框下一步按钮事件
        ceDialogBtn() {
            sessionStorage.removeItem('eCouponGroupMallsList');
            if(this.ceDialogFormData.ceDialogTitle === '电子券新建') {
                if(!this.ceDialogFormData.groupFlag) {
                    this.$openTabSwitch('电子券新建', '/eCouponCreate?webStateFlag=Create&groupFlag=false');
                    this.closeCEDialogVisible();
                } else {
                    if(this.ceDialogFormData.checkList.length < 2) {
                        this.$message({
                            message: '创建集团券必须选择2个或以上商场！',
                            type: 'error'
                        });
                        return false;
                    } else {
                        let params = [];
                        for(let i = 0; i < this.ceDialogFormData.checkList.length; i++) {
                            for(let j = 0; j < this.mallDialogDataList.length; j++) {
                                if(this.ceDialogFormData.checkList[i] === this.mallDialogDataList[j].mallCode) {
                                    params.push({mallCode: this.mallDialogDataList[j].mallCode, mallName: this.mallDialogDataList[j].mallFullName});
                                }
                                continue;
                            }
                        }
                        sessionStorage.setItem('eCouponGroupMallsList', JSON.stringify(params));
                        this.$openTabSwitch('电子券新建', '/eCouponCreate?webStateFlag=Create&groupFlag=true');
                        this.closeCEDialogVisible();
                    }
                }
            }
            // if(this.ceDialogFormData.ceDialogTitle === '电子券新建') {
            //     this.$openTabSwitch('电子券新建', '/eCouponCreate?webStateFlag=Create');
            // } else if(this.ceDialogFormData.ceDialogTitle === '电子券修改') {
            //     this.$openTabSwitch('电子券修改', '/eCouponEdit?webStateFlag=Edit');
            // } else {
            //     return false;
            // }
        },
        // Dialog 对话框关闭事件
        closeCEDialogVisible() {
            this.ceDialogVisible = false;
            this.ceDialogFormData = {
                groupFlag: null,
                checkList: [],
                ceDialogTitle: null
            }
        },
        // 查询电子券批次选择器列表
        queryCouponBatchList(eCouponId) {
            API.getByCouponId(eCouponId, 'true').then((res)=>{
                if(res.data.code === 200) {
                    this.couponBatchOptions = res.data.data;
                    if(this.couponBatchOptions) {
                        this.sendDialogFormData.batchId = this.couponBatchOptions[0].id;
                        this.sendDialogFormData.memberTotal = this.couponBatchOptions[0].maxAcquirableAmount;
                        this.sendDialogFormData.couponTotal = this.couponBatchOptions[0].batchTotalAmount - this.couponBatchOptions[0].grantedAmount - this.couponBatchOptions[0].lockedAmount;
                        this.sendDialogFormData.memberTotalShowFlag = true;
                        this.sendDialogFormData.sendMaxTotalShowFlag = true;
                    }
                }
            }).catch(()=>{

            });
        },
        // 电子券批次选择器 change 事件
        changeBatchIdSelect(val) {
            if(val) {
                for(let i = 0; i < this.couponBatchOptions.length; i++) {
                    if(val === this.couponBatchOptions[i].id) {
                        this.sendDialogFormData.couponTotal = this.couponBatchOptions[i].batchTotalAmount - this.couponBatchOptions[i].grantedAmount - this.couponBatchOptions[i].lockedAmount;
                        break;
                    }
                }
                this.sendDialogFormData.sendMaxTotalShowFlag = true;
            } else {
                this.sendDialogFormData.couponTotal = null;
                this.sendDialogFormData.sendMaxTotalShowFlag = false;
            }
        },
        // 会员手机/会员号输入框 blur 事件
        sendDialogBlurInput(flag) {
            if(flag === 'mobile') {
                if(this.sendDialogFormData.mobile) {
                    this.$refs['sendDialogForm'].clearValidate(['memberCode']);
                }
            }
            if(flag === 'memberCode') {
                if(this.sendDialogFormData.memberCode) {
                    this.$refs['sendDialogForm'].clearValidate(['mobile']);
                }
            }
            let memberMobile = this.sendDialogFormData.mobile ? this.sendDialogFormData.mobile : '';
            let memberCode = this.sendDialogFormData.memberCode ? this.sendDialogFormData.memberCode : '';
            if(memberMobile || memberCode) {
                baseAPI.findMemberDetail(memberMobile, memberCode).then((res)=>{
                    console.log(res);
                    if(res.data.code === 200) {
                        if(res.data.data) {
                            this.sendDialogFormData.memberName = res.data.data.memberName;
                            this.sendDialogFormData.memberFoundFlag = true;
                            this.sendDialogFormData.memberFoundShowFlag = false;
                        } else {
                            this.$message({
                                message: '未查询到有效会员信息！',
                                type: 'error'
                            });
                            this.sendDialogFormData.memberName = null;
                            this.sendDialogFormData.memberFoundFlag = false;
                            this.sendDialogFormData.memberFoundShowFlag = true;
                        }
                    }
                }).catch(()=>{
                    this.sendDialogFormData.memberName = null;
                    this.sendDialogFormData.memberFoundFlag = false;
                    this.sendDialogFormData.memberFoundShowFlag = true;
                });
            } else {
                this.sendDialogFormData.memberName = null;
                this.sendDialogFormData.memberFoundFlag = false;
            }
        },
        // 会员手机输入框 blur 事件
        // blurMobileInput(val) {
        //     if(this.sendDialogFormData.mobile) {
        //         this.$refs['sendDialogForm'].clearValidate(['memberCode']);
        //     }
        //     let memberMobile = this.sendDialogFormData.mobile ? this.sendDialogFormData.mobile : '';
        //     let memberCode = this.sendDialogFormData.memberCode ? this.sendDialogFormData.memberCode : '';
        //     if(memberMobile || memberCode) {
        //         baseAPI.findMemberDetail(memberMobile, memberCode).then((res)=>{
        //             console.log(res);
        //             if(res.data.code === 200) {
        //                 if(res.data.data) {
        //                     this.sendDialogFormData.memberName = res.data.data.memberName;
        //                     this.sendDialogFormData.memberFoundFlag = true;
        //                 } else {
        //                     this.sendDialogFormData.memberFoundFlag = false;
        //                 }
        //             }
        //         }).catch(()=>{
        //             this.sendDialogFormData.memberFoundFlag = false;
        //         });
        //     }
        // },
        // 会员号输入框 blur 事件
        // blurMemberCodeInput(val) {
        //     if(this.sendDialogFormData.memberCode) {
        //         this.$refs['sendDialogForm'].clearValidate(['mobile']);
        //     }
        //     let memberMobile = this.sendDialogFormData.mobile ? this.sendDialogFormData.mobile : '';
        //     let memberCode = this.sendDialogFormData.memberCode ? this.sendDialogFormData.memberCode : '';
        //     if(memberMobile || memberCode) {
        //         baseAPI.findMemberDetail(memberMobile, memberCode).then((res)=>{
        //             console.log(res);
        //             if(res.data.code === 200) {
        //                 if(res.data.data) {
        //                     this.sendDialogFormData.memberName = res.data.data.memberName;
        //                     this.sendDialogFormData.memberFoundFlag = true;
        //                 } else {
        //                     this.sendDialogFormData.memberFoundFlag = false;
        //                 }
        //             }
        //         }).catch(()=>{
        //             this.sendDialogFormData.memberFoundFlag = false;
        //         });
        //     }
        // },
        // Dialog 对话框确定发送按钮事件
        sendDialogBtn() {
            this.$refs['sendDialogForm'].validate((valid) => {
                if (valid) {
                    if(this.sendDialogFormData.memberFoundFlag) {
                        this.sendDialogBtnLoadFlag = true;
                        let params = {
                            couponId: this.sendDialogFormData.eCouponId,
                            couponName: this.sendDialogFormData.eCouponName,
                            batchId: this.sendDialogFormData.batchId,
                            mobile: this.sendDialogFormData.mobile,
                            memberCode: this.sendDialogFormData.memberCode,
                            memberName: this.sendDialogFormData.memberName,
                            grantCouponNum: this.sendDialogFormData.grantCouponNum
                        }
                        API.addSendECouponTask(params).then((res)=>{
                            if(res.data.code === 200) {
                                this.$message({
                                    message: '电子券发放成功！',
                                    type: 'success'
                                });
                                this.sendDialogBtnLoadFlag = false;
                                this.sendDialogVisible = false;
                                this.queryTabData();
                            }
                        }).catch(()=>{
                            this.sendDialogBtnLoadFlag = false;
                        });
                    } else {
                        this.$message({
                            message: '未查询到有效会员信息！',
                            type: 'error'
                        });
                        return false;
                    }
                } else {
                    return false;
                }
            });
        },
        // Dialog 对话框关闭事件
        closeSendDialogVisible() {
            this.sendDialogVisible = false;
            this.couponBatchOptions = [];
            this.sendDialogFormData = {
                eCouponId: null,
                eCouponName: null,
                batchId: null,
                couponTotal: null,
                mobile: null,
                memberCode: null,
                memberName: null,
                grantCouponNum: null,
                sendMaxTotalShowFlag: false,
                memberFoundFlag: false,
                memberFoundShowFlag: false,
                memberTotal: null,
                memberTotalShowFlag: false
            },
            this.$refs['sendDialogForm'].resetFields();
        }
    }
}
</script>

<style scoped lang="less">
.max-h-350 {
    max-height: 350px;
    overflow: auto;
}
.contentDiv {
    .w100 {
        width: 100%;
    }
    .mg-b-0 {
        margin-bottom: 0px;
    }
    .mg-b-24 {
        margin-bottom: 24px;
    }
    .max-h-500 {
        max-height: 500px;
        overflow: auto;
    }
    .textRedColor {
        color: red;
    }
    .formBtnDiv {
        padding-left: 100px;
    }
    .tabBtnDiv {
        position: relative;
        padding-right: 0px;
        .tabBtnSty:not(:first-child) {
            margin-left: 10px!important;
            margin-bottom: 10px;
        }
        .tabControlBtn {
            position: absolute;
            top: 0px;
            right: 0px;
            margin-bottom: 10px;
        }
    }
    .radioTabDiv {
        position: relative;
        .btnMask {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }
    }
    .dialogTitleSty {
        display: flex;
        display: -webkit-flex;
        justify-content: space-between;
        font-size: 16px;
        font-weight: 700;
    }
    .tipPosR {
        position: relative;
        .tipTextPosA {
            position: absolute;
            top: 38px;
            left: 0;
            height: 26px;
            line-height: 26px;
        }
    }
    .formCheckSty {
        /deep/.el-form-item__label:before {
            content: '*';
            color: #FA5B5B;
            margin-right: 4px;
        }
    }
    .el-table {
        /deep/.el-table__expand-icon {
            position: absolute;
            // top: 0px;
            right: 10px;
            font-size: 20px;
            z-index: 999;
        }
    }
}
.goodsDetails {
    font-family: PingFangSC-Regular;
    font-size: 28px;
    color: #000000;
    letter-spacing: 0.74px;
    line-height: 50px;
}
.hr_class {
    height: 10px;
    background: #f5f5f5;
}
.banner {
    display: flex;
    flex-direction: column;
    background: #fff;
}
.banner_img {
    width: 100%;
}
.banner-details {
    padding: 10px 19px 19px 30px;
}
.banner-details_item {
    display: flex;
    justify-content: space-between;
    margin-top: 10px;
}
.banner-details_date {
    font-size: 24px;
    color: rgba(0, 0, 0, 0.25);
    line-height: 30px;
    flex: 1;
}
.banner-details_num {
    font-size: 28px;
    line-height: 40px;
    text-align: left;
    margin-top: 10px;
}
.number {
    color: #fa6400;
}
.buy {
    display: flex;
    background: #fff;
    padding: 24px 19px 24px 30px;
    align-items: center;
}
.buyNum {
    display: flex;
    flex: 1;
    justify-content: flex-end;
    align-items: center;
}
.buyNum_item {
    display: flex;
    justify-content: flex-end;
    align-items: center;
}
.buy_btn {
    width: 52px;
    height: 52px;
    border: none;
    border: 1px solid #979797;
    background: #fff;
    text-align: center;
    line-height: 52px;
    font-size: 30px;
    color: #000000;
    margin-left: 6px;
    cursor: pointer;
}
.buy_input {
    width: 105px;
    height: 52px;
    border: none;
    outline: none;
    border: 1px solid #979797;
    margin-left: 6px;
    text-align: center;
    font-size: 30px;
    color: #000000;
}
.onlyBuy {
    font-size: 26px;
    color: #fa6400;
    margin-right: 20px;
}
.buyList {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 122px;
    font-size: 36px;
    color: #000000;
    line-height: 50px;
    display: flex;
    align-items: center;
    z-index: 100;
    background: #fff;
}
.buyList_item {
    flex: 1;
    padding-left: 33px;
}
.buyNow {
    width: 288px;
    height: 100%;
    color: #fff;
    background: #0091ff;
    text-align: center;
    line-height: 122px;
}
.subtitle {
    padding-left: 30px;
    //margin-top: 10px;
    background: #fff;
}
.subtitle_item {
    padding: 25px 0;
    border-bottom: 1px solid #e8e8e8;
}
.subtitle_text {
    padding: 9px 30px 50px 0;
}
.subtitle_main_text {
    margin-bottom: 27px;
}
.bottomClass {
    margin-bottom: 140px;
}
.codeBox {
    height: 300px;
    text-align: center;
    img {
        width: 300px;

    }
}
.uploadBox {
    text-align: center;
    margin-top: 25px;
}
.diabledBtn {
    /deep/span {
        color: #c0c4cc!important;
    }
}
.redColor {
    color: #fa6400;
}
</style>
