<template>
    <h2 class="top-title">상담배정 내역조회</h2>

    <section class="section">
        <h3 class="sub-title">상담현황</h3>
        <div class="table-wrap type02">
            <table>
                <caption>변호사, 전체, 답변대기, 답변작성중, 답변완료로 구성되어 있음</caption>
                <colgroup>
                    <col width="20%">
                    <col width="20%">
                    <col width="20%">
                    <col width="20%">
                    <col width="*">
                </colgroup>
                <thead>
                    <tr>
                        <th>변호사</th>
                        <th>전체</th>
                        <th>답변대기</th>
                        <th>답변작성중</th>
                        <th>답변완료</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>홍길동</td>
                        <td>25건</td>
                        <td>6건</td>
                        <td>4건</td>
                        <td>10건</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </section>

    <section class="section">
        <h3 class="sub-title">상담조회</h3>

        <div class="search-wrap">
            <div class="search">
                <div class="col">
                    <p class="tit">조회기간</p>
                    <div class="input-wrap date">
                        <input type="date" id="" name="" title="조회 기간 시작 날짜" required="">
                        <img src="../../assets/images/icon_hyphen@3x.png" alt="" class="hyphen">
                        <input type="date" id="" name="" title="조회 기간 시작 날짜" required="">
                    </div>
                </div>
                <div class="col">
                    <p class="tit">답변상태</p>
                    <div class="input-wrap">
                        <select name="" id="">
                            <option value="">전체</option>
                            <option value="">답변대기</option>
                            <option value="">답변작성중</option>
                            <option value="">답변완료</option>
                        </select>
                    </div>
                </div>
            </div>
        </div>
        <div class="btn-wrap mt-50">
            <button type="button" class="btn btn-md btn-blue" onclick="openPopup('pop01')">조회</button>
        </div>
    </section>

    <section class="section">
        <div class="table-wrap">
            <div class="table-top">
                <div class="total-num">
                    총 <span>100</span>건
                </div>
                <div class="btn-wrap right">
                    <button type="button" class="btn btn-consult">대량배정</button>
                    <button type="button" class="btn btn-consult">대량반려</button>
                </div>
            </div>
            <!-- 제목 한 줄로(말줄임) 필요 시 name 클래스에 ellipsis 클래스 추가 -->
            <table>
                <caption>순번, 상담제목, 상담상태, 변호사, 상담요청일, 배정일자 답변일자로 구성됨</caption>
                <colgroup>
                    <col width="80px">
                    <col width="100px">
                    <col width="*">
                    <col width="100px">
                </colgroup>
                <thead>
                    <tr>
                        <th>
                            <div class="common-check all-chk">
                                <label>
                                    <input type="checkbox" name="chk">
                                    <span class="blind">전체선택</span>
                                </label>
                            </div>
                        </th>
                        <th>순번</th>
                        <th>상담제목</th>
                        <th>비고</th>
                    </tr>
                </thead>
                <tbody>
                    <!-- boardList : 해당 열의 추출 값
                    boardLists : 해당 열의 index -->
                    <tr v-for="(boardList, boardLists) in calData" :key="index">
                        <td>
                            <div class="common-check each-chk">
                                <label>
                                    <input type="checkbox" name="chk">
                                    <span class="blind">순번1</span>
                                </label>
                            </div>
                        </td>
                        <td>{{ boardList.id }}</td>
                        <td class="name">
                            <a href="#">{{ boardList.title }}</a>

                        </td>
                        <td>
                            <a href="javascript:void(0)" class="btn btn-more" @click="showTable(); onEdit(boardLists)">상세보기</a>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <v-pagination class="paging-wrap" v-model="curPageNum" :length="numOfPages">
            <!-- <button type="button" class="btn btn-prev02"><span class="blind">5페이지 전으로 가기</span></button> -->
            <button type="button" class="btn btn-prev" @click="prevPage()"><span class="blind">1페이지 전으로
                    가기</span></button>
            <div class="num">
                <a href="javascript:void(0)" v-for="numOfPage in numOfPages" @click="goPage(numOfPage)">{{ numOfPage }}</a>
            </div>
            <button type="button" class="btn btn-next" @click="nextPage()"><span class="blind">1페이지 후로
                    가기</span></button>
            <!-- <button type="button" class="btn btn-next02"><span class="blind">5페이지 후로 가기</span></button> -->
        </v-pagination>
    </section>


    <!-- s:하단 상세보기 -->
    <section class="section" v-if="show">
        <div class="table-wrap type03">
            <div class="btn-wrap right">
                <button type="button" class="btn add close" @click="hideTable">닫기</button>
            </div>
            <table>
                <caption>상담제목, 상담요청일, 상담내용으로 구성됨</caption>
                <colgroup>
                    <col width="280px">
                    <col width="*">
                    <col width="280px">
                    <col width="*">
                </colgroup>

                <tbody>
                    <tr>
                        <th>상담제목</th>
                        <td>
                            <input type="text" class="name" readonly="" v-model="selectedData.title">
                        </td>
                        <th>상담요청일</th>
                        <td class="date">
                            2022.11.28
                        </td>
                    </tr>
                    <tr>
                        <th>상담내용</th>
                        <td colspan="3">
                            <div class="textarea-wrap readonly">
                                <textarea name="inquiry" id="" cols="30" rows="10" readonly="" v-model="selectedData.body"></textarea>
                            </div>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </section>
    <!-- //e:하단 상세보기 -->

</template>

<script>
export default {
    name: "List",
    data() {
        return {
            boardLists: [],
            show: false,
            dataPerPage: 10,
            curPageNum: 1,
            numOfPages: 1,
            curSelectIndex: 0,
            selectedData: {
                title: '',
                body: ''
            }
        }
    },
    mounted() {
        this.getBoardList();
    },
    methods: {
        getBoardList: function () {
            var vm = this;
            this.axios.get('https://jsonplaceholder.typicode.com/posts')
                .then(function (response) {
                    // console.log(response);
                    vm.boardLists = response.data
                })
                .catch(function (error) {
                    // console.log(error);
                });
        },
        showTable(index) {
            this.show = true;
            console.log(index)
        },
        hideTable() {
            this.show = false;
        },
        goPage(index) {
            this.curPageNum = index;
        },
        prevPage() {
            if (this.curPageNum > 1) {
                this.curPageNum--;
            } else {
                alert('첫 번째 페이지입니다.')
            }
        },
        nextPage() {
            if (this.dataPerPage > this.curPageNum) {
                this.curPageNum++;
                
            } else {
                alert('마지막 페이지입니다.');
            }
        },
        onEdit(index) {
            this.curSelectIndex = this.calIndex(index);
            this.selectedData.title = this.boardLists[this.curSelectIndex].title;
            this.selectedData.body = this.boardLists[this.curSelectIndex].body;
        },
        calIndex(index) {
            switch (this.curPageNum) {
                case 1:
                    return index;
                    break;
                default:
                    return index + ((this.curPageNum - 1) * this.dataPerPage);
                    break;
            }
        },
    },
    computed: {
        startOffset() {
            return ((this.curPageNum - 1) * this.dataPerPage);
        },
        endOffset() {
            return (this.startOffset + this.dataPerPage);
        },
        numOfPages() {
            return Math.ceil(this.boardLists.length / this.dataPerPage);
        },
        calData() {
            return this.boardLists.slice(this.startOffset, this.endOffset)
        }
    }
}
</script>