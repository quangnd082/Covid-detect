# CNN-covid
# Hướng dẫn chạy và biên dịch chương trình
# Bước 1: Cài đặt và import các thư viện cần thiết
Ở đoạn mã đầu tiên, chúng ta cần import các thư viện cần thiết cho việc chạy chương trình:
- `glob`: Thư viện `glob` được sử dụng để tìm kiếm các đường dẫn file trong hệ thống tệp tin. Nó được sử dụng để lấy danh sách các file trong một thư mục cụ thể. Trong đề tài này, chúng ta sử dụng bộ dữ liệu covid19 (https://www.kaggle.com/datasets/trangnguyenkieu/covid19)  tham khảo từ bộ dữ liệu COVID-19 Radiography Database (https://www.kaggle.com/datasets/tawsifurrahman/covid19-radiography-database). Bộ dữ liệu được sử dụng đã cắt bớt một số ảnh/folder không cần thiết, chỉ bao gồm 2 folder dữ liệu Covid và Normal 
- `shutil`: Thư viện `shutil` cung cấp các hàm để thao tác với các tệp tin và thư mục, bao gồm sao chép, di chuyển, đổi tên và xóa tệp tin/thư mục.
- `cv2`: Thư viện `cv2` là OpenCV, một thư viện rất mạnh về xử lý ảnh và thị giác máy tính.
- `seaborn`: Thư viện `seaborn` cung cấp các công cụ để vẽ các biểu đồ thống kê và trực quan hóa dữ liệu.
- `numpy`: Thư viện `numpy` cung cấp các công cụ để làm việc với mảng và ma trận nhanh chóng và hiệu quả.
- `pyplot` từ `matplotlib`: Thư viện `pyplot` từ `matplotlib` cung cấp các công cụ để vẽ các biểu đồ và trực quan hóa dữ liệu.
- `tensorflow`: Thư viện `tensorflow` là một thư viện mã nguồn mở rất phổ biến cho học sâu và trí tuệ nhân tạo.
- `tensorflow.keras.layers`: `layers` từ `tensorflow.keras` là một module cung cấp các lớp mô hình học sâu như Convolutional, Dense, Activation, v.v.
- `tensorflow.keras.models`: `models` từ `tensorflow.keras` là một module cung cấp các kiến trúc mô hình học sâu như Sequential, Model, v.v.
- `tensorflow.keras.preprocessing`: `preprocessing` từ `tensorflow.keras` là một module cung cấp các tiện ích tiền xử lý dữ liệu như tải ảnh, chuyển đổi dữ liệu hình ảnh, v.v.
- `keras.applications.vgg16`: `VGG16` từ `keras.applications` là một mô hình học sâu được sử dụng rộng rãi để phân loại hình ảnh.
- `keras`: `keras` là một thư viện mô hình học sâu, nhưng trong đoạn mã trên, được sử dụng từ `tensorflow.keras`.
- `warnings`: Thư viện `warnings` được sử dụng để kiểm soát việc bỏ qua các cảnh báo trong quá trình chạy mã.
- `warnings.filterwarnings('ignore')`: Đoạn mã này tắt cảnh báo để không hiển thị các thông báo cảnh báo trong quá trình thực thi mã.
- 
<img width="667" alt="image" src="https://github.com/NguyenChang21/CNN-covid/assets/95021543/61bb4762-e25e-4494-84fe-f7c5763e3078">

# Bước 2: Tiền xử lý dữ liệu: Dữ liệu của chúng ta cần qua một số bước xử lý trước khi bước vào quá trình huấn luyện: 
- `name_list` sử dụng hàm `glob.glob()` để lấy danh sách đường dẫn của các tệp tin ảnh trong thư mục "/kaggle/input/covid19/COVID-19_Radiography_Dataset/COVID/images/".
- `labels` là danh sách các nhãn tương ứng với các ảnh, trong trường hợp này là ['NORMAL', 'Covid'].
- `X` là danh sách lưu trữ các mảng numpy biểu diễn ảnh.
- `y` là danh sách lưu trữ các nhãn tương ứng với từng ảnh.
Trong vòng lặp, đối với mỗi đường dẫn trong `name_list`, nhãn được thêm vào danh sách `y` (ở đây là nhãn 1 tương ứng với nhãn "Covid"). Ảnh được đọc bằng `cv2.imread()` và chuyển đổi thành mảng numpy bằng `tf.keras.preprocessing.image.img_to_array()`. Sau đó, ảnh được điều chỉnh kích thước thành (128, 128) bằng `cv2.resize()`. Cuối cùng, mảng ảnh được thêm vào danh sách `X`.

 <img width="811" alt="image" src="https://github.com/NguyenChang21/CNN-covid/assets/95021543/016972e7-862e-4f59-b1cb-c64f58bd7db4">
	
Lặp lại tương tự với list normal, thay nhãn =0 tương ứng với nhãn "Normal"

<img width="818" alt="image" src="https://github.com/NguyenChang21/CNN-covid/assets/95021543/4beb4ed1-4f44-4ab4-a002-fb2079390036">

Sau đó, chúng ta chuyển đổi X và y thành dạng array:
- `X` là danh sách các mảng numpy biểu diễn ảnh. Bằng cách sử dụng `np.array(X)`, danh sách này được chuyển đổi thành một mảng numpy đa chiều.
- `y` là danh sách các nhãn tương ứng với từng ảnh. Bằng cách sử dụng `np.array(y)`, danh sách này được chuyển đổi thành một mảng numpy 1 chiều. Hàm `reshape(-1, 1)` được sử dụng để thay đổi hình dạng của mảng từ (n,) thành (n, 1), để đảm bảo rằng mảng `y` có kích thước chính xác và phù hợp với dữ liệu đầu vào của mô hình.
- 
<img width="642" alt="image" src="https://github.com/NguyenChang21/CNN-covid/assets/95021543/eb51891f-ab08-425e-bc65-d0922cc1c49b">

In ra X shape và y shape để kiểm tra:

<img width="644" alt="image" src="https://github.com/NguyenChang21/CNN-covid/assets/95021543/4fbbb7b9-d8f0-4301-af48-cdc85fd15a7d">

Chia tập train, test, validation(val) phục vụ cho việc huấn luyện mô hình, sử dụng hàm train_test_split(X, y, test_size =0.2, random_state=42), với test_size là tỉ lệ của tập test trên toàn bộ đầu vào X, y, random_state được sử dụng để đảm bảo rằng quá trình chia dữ liệu này là nhất quán khi chạy lại chương trình. Sau khi chia, chúng ta có được các tập train, test và val. In ra shape của các tập để kiểm tra.

<img width="684" alt="image" src="https://github.com/NguyenChang21/CNN-covid/assets/95021543/3fbb98e0-077f-4702-9102-94a940677e7a">

Tiếp đó, chúng ta chuẩn hóa dữ liệu đầu vào. Việc chuẩn hóa dữ liệu đóng vai trò quan trọng trong việc cải thiện hiệu suất và ổn định của các thuật toán máy học và học sâu. Nó giúp đưa dữ liệu về cùng một phạm vi giá trị, tăng tốc quá trình học, giảm nhiễu và đảm bảo tính công bằng giữa các đặc trưng.

<img width="573" alt="image" src="https://github.com/NguyenChang21/CNN-covid/assets/95021543/c7493a7d-c60c-4175-a547-cde5f4a34e0e">

# Bước 3: Xây dựng và huấn luyện mô hình:
Mô hình xây dựng dựa trên mô hình VGG16 được cung cấp bởi thư viện Keras. Dòng đầu tiên tạo một instance của mô hình VGG16 bằng cách sử dụng hàm VGG16 từ thư viện Keras. - - Tham số weights='imagenet' cho biết chúng ta sử dụng trọng số đã được huấn luyện trên tập dữ liệu ImageNet. Tham số include_top=False được sử dụng để không bao gồm các tầng Fully Connected Layer cuối cùng của VGG16, bởi vì chúng ta sẽ thêm các tầng Fully Connected Layer riêng cho bài toán của mình. Tham số input_shape=(128, 128, 3) xác định kích thước đầu vào của mô hình là 128x128 pixel với 3 kênh màu (RGB).
- Tiếp theo, đóng băng (freeze) các tầng của mô hình VGG16 bằng cách duyệt qua từng layer trong base_model và gán thuộc tính trainable = False. Điều này đảm bảo rằng các trọng số trong các tầng này sẽ không được cập nhật trong quá trình huấn luyện.
- Sau đó, khởi tạo một mô hình Sequential mới. Thêm base_model vào mô hình bằng cách sử dụng phương thức model.add(base_model). Tiếp theo, thêm một tầng Flatten để chuyển từ tensor 2D thành vector 1D. Điều này là cần thiết để chuẩn bị đầu vào cho các tầng Fully Connected Layer tiếp theo. Thêm tầng Dense với 1024 đơn vị và Activation Function ReLU bằng cách sử dụng model.add(Dense(1024)) và model.add(Activation("relu")). Tương tự, thêm một tầng Dense với 512 đơn vị và kích hoạt ReLU. Cuối cùng, thêm một tầng Dense cuối cùng với một đơn vị và hàm sigmoid, đại diện cho lớp đầu ra nhị phân (0 hoặc 1).
- Cuối cùng, gọi model.summary() để in ra tóm tắt thông tin về kiến trúc của mô hình, bao gồm tên các tầng, kích thước đầu vào và đầu ra, số lượng tham số và thông tin về số lượng trọng số không được huấn luyện.

<img width="637" alt="image" src="https://github.com/NguyenChang21/CNN-covid/assets/95021543/d7f25efb-ac33-48af-9673-9defa9cc9ac1">

Huấn luyện mô hình:
- `EarlyStopping`: Lớp `EarlyStopping` từ `tensorflow.keras.callbacks` được sử dụng để dừng huấn luyện sớm nếu không có cải thiện đáng kể trong mất mát trên tập kiểm tra trong một số lượng epoch liên tiếp (patience).
- `learning_rate`, `decay_steps`, `decay_rate`: Các tham số được sử dụng để khởi tạo learning rate scheduler `lr_scheduler`. `learning_rate` là tốc độ học ban đầu, `decay_steps` là số epoch để áp dụng decay, và `decay_rate` là tỷ lệ giảm learning rate.
- `optimizer1`: Optimizer được khởi tạo bằng cách sử dụng `Adam` optimizer từ `tensorflow.keras.optimizers`, với `learning_rate` được thiết lập bằng `lr_scheduler`.
- `model.compile()`: Cấu hình mô hình với optimizer, hàm mất mát và các metric để đánh giá hiệu suất mô hình.
- `model.fit()`: Huấn luyện mô hình trên dữ liệu huấn luyện (`X_train`, `y_train`) với các tham số như `batch_size`, `epochs`, và sử dụng dữ liệu kiểm tra (`X_val`, `y_val`) để đánh giá hiệu suất của mô hình trong quá trình huấn luyện. Thêm vào đó, `callbacks=[early_stop]` được sử dụng để áp dụng early stopping trong quá trình huấn luyện dựa trên giá trị loss trên tập Validation.

<img width="816" alt="image" src="https://github.com/NguyenChang21/CNN-covid/assets/95021543/0eb8bc21-5add-4468-bd71-fd1a0bde14b8">

In ra đồ thị tương quan giữa giá trị accuracy(độ chính xác) và đồ thị tương quan giữa các giá trị loss trên tập train và validation
- `plt.plot(history.history['accuracy'])` và `plt.plot(history.history['val_accuracy'])`: Vẽ đồ thị độ chính xác trên tập huấn luyện và tập kiểm tra theo các epoch.
- `plt.title('model accuracy')`: Đặt tiêu đề cho đồ thị độ chính xác.
- `plt.ylabel('accuracy')`: Đặt nhãn cho trục y của đồ thị độ chính xác.
- `plt.xlabel('epoch')`: Đặt nhãn cho trục x của đồ thị độ chính xác.
- `plt.legend(['train','test'],loc ='upper left')`: Hiển thị chú thích cho đồ thị, với 'train' là đường màu xanh đại diện cho tập huấn luyện và 'test' là đường màu cam đại diện cho tập kiểm tra.
- `plt.show()`: Hiển thị đồ thị độ chính xác.

Tương tự, các lệnh tiếp theo vẽ đồ thị loss:

- `plt.plot(history.history['loss'])` và `plt.plot(history.history['val_loss'])`: Vẽ đồ thị loss trên tập huấn luyện và tập kiểm tra theo các epoch.
- `plt.title('model loss')`: Đặt tiêu đề cho đồ thị loss.
- `plt.ylabel('loss')`: Đặt nhãn cho trục y của đồ thị loss.
- `plt.xlabel('epoch')`: Đặt nhãn cho trục x của đồ thị loss.
- `plt.legend(['train','val'],loc ='upper left')`: Hiển thị chú thích cho đồ thị, với 'train' là đường màu xanh đại diện cho tập huấn luyện và 'val' là đường màu cam đại diện cho tập kiểm tra.
- `plt.show()`: Hiển thị đồ thị loss.

<img width="816" alt="image" src="https://github.com/NguyenChang21/CNN-covid/assets/95021543/2b692259-ac01-46d8-9d9a-7dd3c6e8f409">

# Bước 4: Đánh giá mô hình trên tập test:
Đầu tiên, chúng ta cần lưu model lại:
- `model.save('model1.h5')` được sử dụng để lưu mô hình vào một tệp tin có tên là "model1.h5". Đây là một cách để lưu trữ mô hình đã được huấn luyện và có thể được sử dụng lại sau này. Tệp tin được lưu dưới định dạng h5, là định dạng chuẩn của h5py, cho phép lưu trữ các đối tượng Keras như mô hình, trọng số, cấu hình và các thuộc tính khác của mô hình.

<img width="465" alt="image" src="https://github.com/NguyenChang21/CNN-covid/assets/95021543/fb900d03-e3df-438d-aed7-2687acf90c14">

Load model đã lưu:

<img width="497" alt="image" src="https://github.com/NguyenChang21/CNN-covid/assets/95021543/34985534-1d7e-4ec3-9504-4e07fc7646ec">

Đánh giá model trên tập test:
- `y_hat = model1.predict(X_test_scaled)`: Sử dụng mô hình đã được lưu trữ (`model1`) để dự đoán nhãn (`y_hat`) cho dữ liệu kiểm tra (`X_test_scaled`).
- `def predict(y_hat)`: Định nghĩa hàm `predict` để ánh xạ giá trị dự đoán (`y_hat`) thành các nhãn dự đoán dạng 0 hoặc 1. Nếu giá trị dự đoán lớn hơn hoặc bằng 0.5, thì được gán nhãn 1, ngược lại, được gán nhãn 0.
- `y_pred = predict(y_hat)`: Áp dụng hàm `predict` để chuyển đổi giá trị dự đoán `y_hat` thành nhãn dự đoán `y_pred`.
- `from sklearn.metrics import accuracy_score`: Nhập hàm `accuracy_score` từ thư viện `sklearn.metrics` để tính toán độ chính xác.
- `accuracy = accuracy_score(y_test, y_pred)`: Tính toán độ chính xác bằng cách so sánh nhãn thực tế `y_test` với nhãn dự đoán `y_pred` và lưu kết quả vào biến `accuracy`.
- `print(accuracy)`: In ra giá trị độ chính xác trên tập kiểm tra.

<img width="821" alt="image" src="https://github.com/NguyenChang21/CNN-covid/assets/95021543/5086d039-9b8d-4e9c-851d-92ae3b7f6252">

Gán nhãn cho giá trị y_pred và y_test lần lượt vào các tập result và real_result để in ra kết quả

<img width="814" alt="image" src="https://github.com/NguyenChang21/CNN-covid/assets/95021543/0d9cf848-c54b-4d01-8c75-8954205e4a0e">

In ra classification report và accuracy của mô hình
- `from sklearn.metrics import confusion_matrix, classification_report, accuracy_score`: Nhập các hàm `confusion_matrix`, `classification_report`, `accuracy_score` từ thư viện `sklearn.metrics`.
- `labels = ['Covid', 'Normal']`: Định nghĩa nhãn cho các lớp dự đoán.
- `report = classification_report(y_test, y_pred, target_names=labels)`: Tính toán báo cáo phân loại dựa trên nhãn thực tế `y_test` và nhãn dự đoán `y_pred`, với `target_names` là danh sách các nhãn của các lớp.
- `print(report)`: In ra báo cáo phân loại, bao gồm các thông số như precision, recall, f1-score và support cho từng lớp.
- `accuracy = accuracy_score(y_test, y_pred)`: Tính toán độ chính xác bằng cách so sánh nhãn thực tế `y_test` với nhãn dự đoán `y_pred` và lưu kết quả vào biến `accuracy`.
- `print(f"Accuracy: {accuracy}")`: In ra giá trị độ chính xác trên tập kiểm tra.

<img width="803" alt="image" src="https://github.com/NguyenChang21/CNN-covid/assets/95021543/73f85c8a-62fa-4a70-abb1-447dd3924e4b">

In ra confusion matrix:
- `cm = confusion_matrix(y_test, y_pred)`: Tính toán ma trận nhầm lẫn (confusion matrix) dựa trên nhãn thực tế `y_test` và nhãn dự đoán `y_pred`, và lưu kết quả vào biến `cm`.
- `print(cm)`: In ra ma trận nhầm lẫn.
- `sns.heatmap(cm, annot=True, fmt='d', cmap='Blues')`: Vẽ biểu đồ heatmap sử dụng thư viện Seaborn để hiển thị ma trận nhầm lẫn `cm`. Tham số `annot=True` dùng để hiển thị giá trị trong từng ô, `fmt='d'` để định dạng giá trị là số nguyên, và `cmap='Blues'` để chọn màu sắc cho biểu đồ.
- `tick_labels = ['Covid', 'Normal']`: Định nghĩa nhãn cho các trục x và y.
- `plt.xticks(np.arange(len(tick_labels)) + 0.5, tick_labels)`: Đặt nhãn cho các điểm trên trục x.
- `plt.yticks(np.arange(len(tick_labels)) + 0.5, tick_labels)`: Đặt nhãn cho các điểm trên trục y.
- `plt.xlabel('Predicted')` và `plt.ylabel('True')`: Đặt tên cho trục x và trục y.
- `plt.title('Confusion Matrix')`: Đặt tiêu đề cho biểu đồ.
- `plt.show()`: Hiển thị biểu đồ ma trận nhầm lẫn.

<img width="721" alt="image" src="https://github.com/NguyenChang21/CNN-covid/assets/95021543/cb8fc1ad-0704-45a1-8acc-1acc7c1fbea2">

In ra các ảnh X_ray tương ứng cùng với nhãn dự đoán và nhãn thực tế:
- `import matplotlib.pyplot as plt`: Nhập thư viện `matplotlib.pyplot` để hiển thị ảnh và kết quả dự đoán.
- `def show_image_with_prediction(image_array, prediction, label)`: Định nghĩa hàm `show_image_with_prediction` nhận đầu vào là mảng ảnh `image_array`, kết quả dự đoán `prediction` và nhãn thực tế `label`.
- `plt.imshow(image_array)`: Hiển thị ảnh từ mảng `image_array`.
- `plt.axis('off')`: Tắt hiển thị các trục.
- `plt.title(f'Prediction: {prediction}, Label: {label}')`: Đặt tiêu đề cho ảnh, gồm kết quả dự đoán và nhãn thực tế.
- `plt.show()`: Hiển thị ảnh và tiêu đề.
- `image_arrays = X_test`: Gán mảng các ảnh kiểm tra vào biến `image_arrays`.
- `predictions = result`: Gán mảng các kết quả dự đoán vào biến `predictions`.
- `label = real_result`: Gán mảng các nhãn thực tế vào biến `label`.
- `for image_array, prediction, label in zip(image_arrays, predictions, label):`: Lặp qua từng cặp ảnh, kết quả dự đoán và nhãn thực tế.
- `show_image_with_prediction(image_array/255, prediction, label)`: Gọi hàm `show_image_with_prediction` để hiển thị ảnh và kết quả dự đoán.

<img width="839" alt="image" src="https://github.com/NguyenChang21/CNN-covid/assets/95021543/cd41a529-3bd7-42ac-82df-2f2c67a794f7">


Link Kaggle: https://www.kaggle.com/code/trangnguyenkieu/vgg16-final

