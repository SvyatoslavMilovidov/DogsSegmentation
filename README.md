# Семантическая сегментация

* #### __Решаемая задача__ — разработать telegram-бота для удаления заднего фона на фотографиях с собаками.
* #### __Используемый стек__ — `Python` `PyTorch` `Tensorflow` `Aiogram`

## Структура проекта

* model: содержит ноутбук с обучением модели

## Выбор датасета

Для работы были выбраны два датасета:
    - [Dog Segmentation Dataset](https://www.kaggle.com/datasets/santhoshkumarv/dog-segmentation-dataset)
    - [OxfordIIITPet]([https://www.kaggle.com/datasets/santhoshkumarv/dog-segmentation-dataset](https://www.kaggle.com/datasets/tanlikesmath/the-oxfordiiit-pet-dataset))

Первый использовался для тренировки работы с кастомный датасетом \(160 изображений\). Из-за небольшого количества данных, для дальнешей работы использовался второй датасет.

## Подготовка данных

OxfordIIITPet можно выгрузить напрямую из `torchvision`, а для Dog Segmentation Dataset необходимо создать свой класс датасета. Данные этого датасета хранятся в виде изображений и масок к ним. Исходные изображения имеют 3 канала RGB, а выходная маска 1 канал. Изображения име.т разный размер, поэтому необходимо дополнительно их промасштабировать.

Реализованный класс загружает наименование изображений и масок и при обращении за новой парой данных, выгружает картинки, пропуская через трансформер с ресайзом. Таким образом, входное и выходное изображения кодируются в тензоры 3\*512\*512 и 1\*512\*512 соответственно.
   
## Архитектура сети

В качестве архитектуры была выбрана модель U-Net с 5-ю блоками кодирования и декодирования. Каждый блок состоит из двух блоков свертки, батч-нормализации и функции-активации.

## Обучение модели

Функционалом качества был выбран `CrossEntropyLoss`. Обчение модели на датасете Dog Segmentation Dataset показала низкий результат, что ожидаемо, ведь в датасете всего 160 изображений. Обучение на OxfordIIITPet показало результат в \~83% \(при 4-х эпохах обучения\), что хаметно лучше предыдущей версии.

## Разработка telegram-бота

Следующий этап - разработка telegram-бота на aiogram. Пользователь отправляет изображения, а в ответ получает фотографию с вырезанным заданим фоном. **Этот этап находится в разработке**
