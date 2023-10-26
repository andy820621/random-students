<template>
	<div class="main-content">
		<div class="img-box">
			<!-- 背景圖 -->
			<img class="main-img" src="./assets/background-school.jpg" alt="" />

			<!-- 當前亂數結果 -->
			<div class="current-result">
				<p v-if="alertMessage" class="alert">{{ alertMessage }}</p>

				<ul v-else class="schoolContainer">
					<li v-for="(students, schoolName) in showData" :key="schoolName">
						<p class="schoolName">{{ schoolName }}</p>
						<ul class="students">
							<li
								class="student"
								v-for="studentName in students"
								:key="studentName"
							>
								{{ studentName }}
							</li>
						</ul>
					</li>
				</ul>
			</div>

			<!-- 起動亂數 -->
			<div class="btn" @click.prevent="generateRandomNumbers">
				<img src="./assets/logoButton.png" alt="" />
			</div>

			<!-- 亂數數字選擇 -->
			<div class="inputBox">
				<label for="count">數字: </label>
				<input id="count" type="number" v-model="count" max="20" />
			</div>

			<!-- 重新開始 -->
			<button class="restart" @click="confirmRestart">重新開始</button>
		</div>
	</div>
</template>

<script setup lang="ts">
import { ref, watch, onMounted, computed } from "vue";
const startNumber = ref(1);
const endNumber = ref(41);
const numberArray = ref<number[]>([]);
const stringArray = ref<{ schoolName: string; studentName: string }[]>([
	{
		schoolName: "後埔國民小學",
		studentName: "廖晨瑄",
	},
	{
		schoolName: "後埔國民小學",
		studentName: "張桉緁",
	},
	{
		schoolName: "後埔國民小學",
		studentName: "鄭伈妤",
	},
	{
		schoolName: "後埔國民小學",
		studentName: "李語希",
	},
	{
		schoolName: "後埔國民小學",
		studentName: "范馨予",
	},
	{
		schoolName: "後埔國民小學",
		studentName: "張瑋珊",
	},
	{
		schoolName: "後埔國民小學",
		studentName: "郭文逸",
	},
	{
		schoolName: "後埔國民小學",
		studentName: "張元滐",
	},
	{
		schoolName: "後埔國民小學",
		studentName: "莊庭瑀",
	},
	{
		schoolName: "後埔國民小學",
		studentName: "陳澐禛",
	},
	{
		schoolName: "埔墘國民小學",
		studentName: "吳恩辰",
	},
	{
		schoolName: "埔墘國民小學",
		studentName: "胡圉筠",
	},
	{
		schoolName: "埔墘國民小學",
		studentName: "吳璨華",
	},
	{
		schoolName: "埔墘國民小學",
		studentName: "鄭又慈",
	},
	{
		schoolName: "埔墘國民小學",
		studentName: "林詩婷",
	},
	{
		schoolName: "埔墘國民小學",
		studentName: "邱徐楷",
	},
	{
		schoolName: "埔墘國民小學",
		studentName: "吳承諭",
	},
	{
		schoolName: "埔墘國民小學",
		studentName: "林庭宇",
	},
	{
		schoolName: "埔墘國民小學",
		studentName: "王泓翰",
	},
	{
		schoolName: "埔墘國民小學",
		studentName: "陳沛瑄",
	},
	{
		schoolName: "重慶國民小學",
		studentName: "呂逸宏",
	},
	{
		schoolName: "重慶國民小學",
		studentName: "王浩宇",
	},
	{
		schoolName: "重慶國民小學",
		studentName: "莊李婕",
	},
	{
		schoolName: "重慶國民小學",
		studentName: "陳嘉瑋",
	},
	{
		schoolName: "重慶國民小學",
		studentName: "甘依璇",
	},
	{
		schoolName: "重慶國民小學",
		studentName: "巫承諺",
	},
	{
		schoolName: "溪洲國民小學",
		studentName: "劉佩瑄",
	},
	{
		schoolName: "溪洲國民小學",
		studentName: "蕭凱裕",
	},
	{
		schoolName: "溪洲國民小學",
		studentName: "潘奕璇",
	},
	{
		schoolName: "溪洲國民小學",
		studentName: "董彦寬",
	},
	{
		schoolName: "溪洲國民小學",
		studentName: "傅詠勳",
	},
	{
		schoolName: "溪洲國民小學",
		studentName: "吳睿庠",
	},
	{
		schoolName: "溪洲國民小學",
		studentName: "林旻瑜",
	},
	{
		schoolName: "溪洲國民小學",
		studentName: "洪詠翔",
	},
	{
		schoolName: "溪洲國民小學",
		studentName: "賴宥仁",
	},
	{
		schoolName: "溪洲國民小學",
		studentName: "李鎧羽",
	},
	{
		schoolName: "信義國民小學",
		studentName: "段宥臣",
	},
	{
		schoolName: "信義國民小學",
		studentName: "潘芊妤",
	},
	{
		schoolName: "信義國民小學",
		studentName: "李翊汝",
	},
	{
		schoolName: "信義國民小學",
		studentName: "顏苡安",
	},
	{
		schoolName: "信義國民小學",
		studentName: "林志偉",
	},
	{
		schoolName: "信義國民小學",
		studentName: "林侑璇",
	},
	{
		schoolName: "信義國民小學",
		studentName: "邱奕菲",
	},
	{
		schoolName: "信義國民小學",
		studentName: "顏菱宣",
	},
	{
		schoolName: "信義國民小學",
		studentName: "陳家佑",
	},
	{
		schoolName: "信義國民小學",
		studentName: "徐珞棠",
	},
	{
		schoolName: "實踐國民小學",
		studentName: "蔡憶瑄",
	},
	{
		schoolName: "實踐國民小學",
		studentName: "吳羿蓁",
	},
	{
		schoolName: "實踐國民小學",
		studentName: "潘晨允",
	},
	{
		schoolName: "實踐國民小學",
		studentName: "陳妍伶",
	},
	{
		schoolName: "實踐國民小學",
		studentName: "黃于恩",
	},
	{
		schoolName: "實踐國民小學",
		studentName: "吳宓慈",
	},
	{
		schoolName: "實踐國民小學",
		studentName: "溫語彤",
	},
	{
		schoolName: "實踐國民小學",
		studentName: "李品湄",
	},
	{
		schoolName: "實踐國民小學",
		studentName: "鄭昀錡",
	},
	{
		schoolName: "大觀國民小學",
		studentName: "廖婕喬",
	},
	{
		schoolName: "大觀國民小學",
		studentName: "張葦苓",
	},
	{
		schoolName: "大觀國民小學",
		studentName: "唐  芯",
	},
	{
		schoolName: "大觀國民小學",
		studentName: "溫芸莛",
	},
	{
		schoolName: "大觀國民小學",
		studentName: "吳郁柔",
	},
	{
		schoolName: "大觀國民小學",
		studentName: "簡妤芯",
	},
	{
		schoolName: "大觀國民小學",
		studentName: "蔡芯瑜",
	},
	{
		schoolName: "大觀國民小學",
		studentName: "劉禹岑",
	},
	{
		schoolName: "大觀國民小學",
		studentName: "沈梓玉",
	},
	{
		schoolName: "文德國民小學",
		studentName: "李坤儒",
	},
	{
		schoolName: "文德國民小學",
		studentName: "張宸源",
	},
	{
		schoolName: "文德國民小學",
		studentName: "蔡羽鈊",
	},
	{
		schoolName: "文德國民小學",
		studentName: "封立杰",
	},
	{
		schoolName: "文德國民小學",
		studentName: "陳立謙",
	},
	{
		schoolName: "文德國民小學",
		studentName: "蔡昀霏",
	},
	{
		schoolName: "文德國民小學",
		studentName: "阮日瑜",
	},
	{
		schoolName: "文德國民小學",
		studentName: "趙品証",
	},
	{
		schoolName: "文德國民小學",
		studentName: "林安彤",
	},
	{
		schoolName: "文德國民小學",
		studentName: "楊沛媞",
	},
	{
		schoolName: "沙崙國民小學",
		studentName: "詹宸語",
	},
	{
		schoolName: "沙崙國民小學",
		studentName: "高芷芹",
	},
	{
		schoolName: "沙崙國民小學",
		studentName: "許欣語",
	},
	{
		schoolName: "沙崙國民小學",
		studentName: "黃熙元",
	},
	{
		schoolName: "沙崙國民小學",
		studentName: "籃子漢",
	},
	{
		schoolName: "沙崙國民小學",
		studentName: "滕永福",
	},
	{
		schoolName: "沙崙國民小學",
		studentName: "莊怡凡",
	},
	{
		schoolName: "板橋國民小學",
		studentName: "李宇芹",
	},
	{
		schoolName: "板橋國民小學",
		studentName: "黃慕潔",
	},
	{
		schoolName: "板橋國民小學",
		studentName: "陳思瑜",
	},
	{
		schoolName: "板橋國民小學",
		studentName: "潘芃秀",
	},
	{
		schoolName: "板橋國民小學",
		studentName: "俞景淏",
	},
	{
		schoolName: "板橋國民小學",
		studentName: "張子淳",
	},
	{
		schoolName: "板橋國民小學",
		studentName: "黃俞昌",
	},
	{
		schoolName: "板橋國民小學",
		studentName: "李品儀",
	},
	{
		schoolName: "板橋國民小學",
		studentName: "林佳臻",
	},
	{
		schoolName: "板橋國民小學",
		studentName: "李嘉承",
	},
	{
		schoolName: "江翠國民小學",
		studentName: "李秉翰",
	},
	{
		schoolName: "江翠國民小學",
		studentName: "李咏霖",
	},
	{
		schoolName: "江翠國民小學",
		studentName: "蕭子傑",
	},
	{
		schoolName: "江翠國民小學",
		studentName: "林瑋成",
	},
	{
		schoolName: "江翠國民小學",
		studentName: "蔡鉉溰",
	},
	{
		schoolName: "江翠國民小學",
		studentName: "陳宥均",
	},
	{
		schoolName: "江翠國民小學",
		studentName: "劉佳斌",
	},
	{
		schoolName: "江翠國民小學",
		studentName: "陳廉凱",
	},
	{
		schoolName: "江翠國民小學",
		studentName: "余宣荷",
	},
	{
		schoolName: "江翠國民小學",
		studentName: "黃靖岐",
	},
	{
		schoolName: "中山國民小學",
		studentName: "張皓揚",
	},
	{
		schoolName: "中山國民小學",
		studentName: "張梓晴",
	},
	{
		schoolName: "中山國民小學",
		studentName: "江念恩",
	},
	{
		schoolName: "中山國民小學",
		studentName: "王姵穎",
	},
	{
		schoolName: "中山國民小學",
		studentName: "張芷瑜",
	},
	{
		schoolName: "中山國民小學",
		studentName: "江雨涵",
	},
	{
		schoolName: "中山國民小學",
		studentName: "陳雅昂",
	},
	{
		schoolName: "中山國民小學",
		studentName: "梁紫玹",
	},
	{
		schoolName: "中山國民小學",
		studentName: "劉宣岑",
	},
	{
		schoolName: "中山國民小學",
		studentName: "李思潔",
	},
	{
		schoolName: "新埔國民小學",
		studentName: "陳欣沛",
	},
	{
		schoolName: "新埔國民小學",
		studentName: "劉俊航",
	},
	{
		schoolName: "新埔國民小學",
		studentName: "吳哲安",
	},
	{
		schoolName: "新埔國民小學",
		studentName: "侯奕慈",
	},
	{
		schoolName: "新埔國民小學",
		studentName: "林雨瑩",
	},
	{
		schoolName: "新埔國民小學",
		studentName: "蕭于哲",
	},
	{
		schoolName: "新埔國民小學",
		studentName: "許荇壹",
	},
	{
		schoolName: "新埔國民小學",
		studentName: "黃惜初",
	},
	{
		schoolName: "新埔國民小學",
		studentName: "蘇亦辰",
	},
	{
		schoolName: "新埔國民小學",
		studentName: "熊賢永",
	},
	{
		schoolName: "莒光國民小學",
		studentName: "林妤蓁",
	},
	{
		schoolName: "莒光國民小學",
		studentName: "王禹樂",
	},
	{
		schoolName: "莒光國民小學",
		studentName: "蔡依宸",
	},
	{
		schoolName: "莒光國民小學",
		studentName: "洪玉庭",
	},
	{
		schoolName: "莒光國民小學",
		studentName: "王耀宏",
	},
	{
		schoolName: "莒光國民小學",
		studentName: "凌凱喆",
	},
	{
		schoolName: "莒光國民小學",
		studentName: "李彤",
	},
	{
		schoolName: "莒光國民小學",
		studentName: "潘歆霓",
	},
	{
		schoolName: "莒光國民小學",
		studentName: "陳信源",
	},
	{
		schoolName: "莒光國民小學",
		studentName: "吳宇翔",
	},
	{
		schoolName: "文聖國民小學",
		studentName: "李翊瀚 ",
	},
	{
		schoolName: "文聖國民小學",
		studentName: "林則丞",
	},
	{
		schoolName: "文聖國民小學",
		studentName: "許芷綾",
	},
	{
		schoolName: "文聖國民小學",
		studentName: "史聖安",
	},
	{
		schoolName: "文聖國民小學",
		studentName: "張育維",
	},
	{
		schoolName: "文聖國民小學",
		studentName: "彭俐漩",
	},
	{
		schoolName: "文聖國民小學",
		studentName: "陳亞溱",
	},
	{
		schoolName: "文聖國民小學",
		studentName: "陳韋彤",
	},
	{
		schoolName: "文聖國民小學",
		studentName: "粘家甄",
	},
	{
		schoolName: "文聖國民小學",
		studentName: "柯曉鏇",
	},
	{
		schoolName: "海山國民小學",
		studentName: "林莉橙",
	},
	{
		schoolName: "海山國民小學",
		studentName: "李奕璇",
	},
	{
		schoolName: "海山國民小學",
		studentName: "林宥彤",
	},
	{
		schoolName: "海山國民小學",
		studentName: "曾品豪",
	},
	{
		schoolName: "海山國民小學",
		studentName: "趙睿騰",
	},
	{
		schoolName: "海山國民小學",
		studentName: "李昕潼",
	},
	{
		schoolName: "海山國民小學",
		studentName: "林莉潼",
	},
	{
		schoolName: "海山國民小學",
		studentName: "翁以倢",
	},
	{
		schoolName: "海山國民小學",
		studentName: "王渝晴",
	},
	{
		schoolName: "海山國民小學",
		studentName: "王渝翔",
	},
	{
		schoolName: "國光國民小學",
		studentName: "吳孟玹",
	},
	{
		schoolName: "國光國民小學",
		studentName: "林嬡婷",
	},
	{
		schoolName: "國光國民小學",
		studentName: "陳佳妮",
	},
]);
endNumber.value = stringArray.value.length;
for (let i = startNumber.value; i <= endNumber.value; i++) {
	numberArray.value.push(i);
}
console.log("numberArray", numberArray.value.length);

const count = ref(10);
const randomNumbers = ref<number[]>([]);
const selectedNumbersArray = ref<number[][]>([]);
const alertMessage = computed(() => (numberArray.value.length ? "" : "結束"));

type GroupedData = {
	[key: string]: string[];
};

const showData = computed(() => {
	const seletedData = randomNumbers.value.map(
		(num: number) => stringArray.value[num - 1]
	);

	return seletedData.reduce((acc: GroupedData, item) => {
		const school = item.schoolName;
		const student = item.studentName;

		// If the school does not exist in the accumulator object yet, initialize it with an empty array
		if (!acc[school]) {
			acc[school] = [];
		}

		// Push the student to the school's array
		acc[school].push(student);

		return acc;
	}, {});
});

watch(
	showData,
	(val) => {
		console.log("showData", val);
		console.log("selectedNumbersArray", selectedNumbersArray.value);
		console.log("numberArray length", numberArray.value.length);
	},
	{ deep: true }
);

onMounted(() => {
	const numsArray = localStorage.getItem("selectedNumbersArray");
	if (numsArray) {
		let storageCount = 0;
		JSON.parse(numsArray).forEach((numsArr: number[]) => {
			numsArr.forEach((num: number) => {
				storageCount++;
			});
		});
		if (storageCount === endNumber.value - startNumber.value + 1) {
			numberArray.value.length = 0;
		}
		selectedNumbersArray.value = JSON.parse(numsArray);
	}
});

const generateRandomNumbers = () => {
	if (alertMessage.value.length) return;

	randomNumbers.value = [];

	let schoolArray: { [schoolName: string]: string[] } = {};

	let maxWhileCount = 1000;

	while (
		randomNumbers.value.length < count.value &&
		numberArray.value.length > 0
	) {
		maxWhileCount--;

		const randomIndex = Math.floor(Math.random() * numberArray.value.length);

		// 先暫存選中的數字
		const tentativeSelectedNumber = numberArray.value[randomIndex];
		const { schoolName, studentName } =
			stringArray.value[tentativeSelectedNumber - 1];

		if (
			Object.keys(schoolArray).includes(schoolName) ||
			Object.keys(schoolArray).length < 6
		) {
			// 若滿足條件，再從 numberArray 中移除此數字
			const selectedNumber = numberArray.value.splice(randomIndex, 1)[0];
			if (Object.keys(schoolArray).includes(schoolName)) {
				schoolArray[schoolName].push(studentName);
			} else {
				schoolArray[schoolName] = [studentName];
			}
			randomNumbers.value.push(selectedNumber);
		}

		if (maxWhileCount <= 0) {
			const selectedNumber = numberArray.value.splice(randomIndex, 1)[0];
			schoolArray[schoolName] = [studentName];
			randomNumbers.value.push(selectedNumber);
			maxWhileCount = 1000;
		}
	}

	// 添加到 selectedNumbersArray 中
	selectedNumbersArray.value.push(randomNumbers.value);

	// 更新本地存储
	localStorage.setItem(
		"selectedNumbersArray",
		JSON.stringify(selectedNumbersArray.value)
	);
};

function confirmRestart() {
	const shouldRestart = window.confirm("是否要重新開始？");
	if (shouldRestart) {
		localStorage.removeItem("selectedNumbersArray");
		window.location.reload();
	}
}
</script>

<style scoped lang="scss">
.main-content {
	width: 100%;
	height: 100vh;
	display: flex;
	justify-content: center;
	.img-box {
		position: relative;
		.current-result,
		.circle,
		.btn,
		.inputBox,
		.restart {
			position: absolute;
		}
		.restart {
			bottom: 2.4%;
			right: 2.4%;
			transform: translateX(-50%);
			z-index: 3;
			// 按鈕樣式
			background-color: #ff6600;
			color: #fff;
			padding: 10px 20px;
			border: none;
			border-radius: 5px;
			cursor: pointer;
			transition: background-color 0.3s ease;

			&:hover {
				background-color: #ff8800;
			}

			&:active {
				background-color: #ff4400;
				transform: translateX(-50%) scale(0.95);
			}
		}
		.inputBox {
			width: 10%;
			height: 100px;
			left: 9%;
			bottom: 83%;
			z-index: 3;
			display: flex;
			align-items: center;
			justify-content: flex-start;
			label {
				// display: none;
				font-size: 1rem;
			}
			input {
				text-align: center;
				width: 34%;
				border: none;
				outline: none;
				padding: 0;
				margin: 0;
				background: transparent;
				font-size: 2rem;
				color: #000;
			}
			// input[type="number"]::-webkit-inner-spin-button,
			// input[type="number"]::-webkit-outer-spin-button {
			// -webkit-appearance: none;
			// appearance: none;
			// border: 1px solid #000;
			// display: none;
			// }
		}
		.btn {
			width: 328px;
			bottom: 8px;
			left: 50%;
			transform: translateX(-160px);
			z-index: 3;
			cursor: pointer;
			backdrop-filter: blur(2.4px);
			&:active {
				transform: translateX(-160px) translateY(0.1rem);
			}
			img {
				width: 100%;
				object-fit: cover;
				object-position: center;
			}
		}
		.current-result {
			width: 82%;
			height: 55%;
			top: 29%;
			left: 50%;
			transform: translateX(-50%);
			z-index: 3;
			font-family: Arial, sans-serif;
			// border: 1px solid #ddd;
			// border-radius: 8px;
			// box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
			list-style: none;
			overflow: auto;
			display: grid;
			place-items: center;
			.alert {
				text-align: center;
				font-size: 2rem;
				// margin-top: ;
			}

			p {
				font-weight: bold;
				margin: 0;
			}

			.schoolContainer {
				display: flex;
				flex-wrap: wrap;
				row-gap: 0.5rem;
				column-gap: 1rem;
				justify-content: center;
				text-align: center;
				margin: 2.4px 0;

				& > li {
					flex-basis: calc(33.333% - 1rem);
					margin: 0;
					margin-bottom: 2.4rem;
					.schoolName {
						font-size: 1.5em;
						font-weight: bold;
						background-color: rgb(179, 152, 74);
						color: white;
						padding: 10px 20px;
						border-radius: 5px;
						box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
						margin-bottom: 20px;
					}

					.students {
						display: grid;
						justify-content: center;
						grid-template-columns: repeat(3, 1fr);
						gap: 1rem;

						.student {
							// margin: 5px 0;
							font-size: 1.8rem;
							background-color: #f5f5f5;
							padding: 5px 0.1vw;
							border: 1px solid #ddd;
							border-radius: 5rem;
							box-shadow: 1px 1px 3px rgba(0, 0, 0, 0.1);
						}
					}
				}
			}
		}

		.circle {
			position: absolute;
			width: 49.8%;
			aspect-ratio: 1;
			top: 3.1%;
			left: 3.3%;
			z-index: 1;
			background-image: linear-gradient(
				90deg,
				#fffbc6 0%,
				#fff22c 50%,
				#fff100 100%
			);
			border-radius: 50%;
			overflow: hidden;

			.selected-number-array {
				width: 50%;
				min-width: 410px;
				margin: 5rem auto;
				height: calc(100% - 10rem);
				display: flex;
				flex-direction: column;
				justify-content: flex-start;
				align-items: center;
				overflow: auto;
				& > li {
					display: flex;
					align-items: center;
					gap: 1rem;
				}
			}
		}
		.main-img {
			position: relative;
			height: 100%;
			object-fit: cover;
			object-position: center;
			z-index: 2;
			pointer-events: none;
		}
	}
}

ul {
	list-style-type: none;
	padding: 0;
}

li {
	margin: 5px 0;
	font-size: 1.5rem;
}
</style>
