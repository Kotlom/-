# -circleproduct: 
package com.example.pr2;

import javafx.scene.layout.AnchorPane;
import javafx.scene.paint.Paint;
import javafx.scene.shape.Circle;

public class CircleProduct implements ShapeProduct {
    @Override
    public void setup(AnchorPane pane, double x, double y, Paint color, double strokeWidth, HelloController controller) {
        Circle circle = new Circle(x, y, 0);
        circle.setFill(color);
        circle.setOnScroll(controller::OnScroll);
        circle.setOnMouseClicked(controller::OnMouseClicked);

        pane.getChildren().add(circle);

        // ВАЖНО: передаем ссылку назад в контроллер, чтобы работал panedragged
        controller.setCIRCLE(circle);
    }
}

helloaplication: 
package com.example.pr2;

import javafx.application.Application;
import javafx.fxml.FXMLLoader;
import javafx.scene.Scene;
import javafx.stage.Stage;

import java.io.IOException;

public class HelloApplication extends Application {
    @Override
    public void start(Stage stage) throws IOException {
        FXMLLoader fxmlLoader = new FXMLLoader(HelloApplication.class.getResource("hello-view.fxml"));
        Scene scene = new Scene(fxmlLoader.load(), 600, 400);
        stage.setTitle("Hello!");
        stage.setScene(scene);
        stage.show();
    }
}

hellocontroller:
package com.example.pr2;

import javafx.fxml.FXML;
import javafx.scene.Group;
import javafx.scene.PerspectiveCamera;
import javafx.scene.input.MouseButton;
import javafx.scene.input.MouseEvent;
import javafx.scene.control.Slider;
import javafx.scene.input.ScrollEvent;
import javafx.scene.AmbientLight;
import javafx.scene.PointLight;
import javafx.scene.paint.Color;
import javafx.scene.paint.PhongMaterial;
import javafx.scene.shape.Circle;
import javafx.scene.shape.Line;
import javafx.scene.control.Tab;
import javafx.scene.shape.Rectangle;
import javafx.scene.shape.Shape;
import javafx.scene.control.MenuItem;
import java.util.Random;
import javafx.scene.paint.Paint;
import javafx.scene.layout.AnchorPane;
import javafx.event.ActionEvent;
import javafx.scene.shape.Box;

import javafx.scene.control.SplitMenuButton;
import javafx.scene.transform.Rotate;
import javafx.scene.transform.Translate;

public class HelloController {

//3 пр

    @FXML
    private AnchorPane pane3;
    @FXML
    private Tab tab3;
    @FXML
    private Box box;
    private Rotate rotateXAsis;
    private Rotate rotateYAsis;
    private Translate translate;
    private PhongMaterial material = new PhongMaterial();
    private final double movementSpeed = 10.0;
    private double mouseOldX, mouseOldY;
    private double mouseDeltaX, mouseDeltaY;
    private PerspectiveCamera camera;
    private final double rotationSpeed = 45.0;
    Group modelGroup = new Group();
    private final double mouseSensitivity = 0.1;
    //


    @FXML
    private Circle CIRCLE;

    @FXML
    private Line LINE;
    Double startMouseX;
    Double startMouseY;
    Double endMouseX;
    Double endMouseY;

    @FXML
    private Rectangle RECTANGLE;

    @FXML
    private Slider SLIDER;

    @FXML
    private MenuItem actionGREEN;
    private Paint color1;

    @FXML
    private MenuItem actionCIRCLE;

    @FXML
    private MenuItem actionLINE;

    @FXML
    private MenuItem actionRECTANGLE;

    @FXML
    private MenuItem actionRed;

    @FXML
    private MenuItem actionblue;

    @FXML
    private SplitMenuButton figura;
    @FXML
    private AnchorPane pane;

    @FXML
    private AnchorPane pane1;

    @FXML
    private SplitMenuButton svet;

    private double startAngle;

    private double anchorX;
    private double anchorY;

    Random random = new Random();

    private Color generateRandomColor(Random random) {
        return Color.rgb(random.nextInt(256), random.nextInt(256), random.nextInt(256));
    }
    private Color determineColor(String text) {
        return switch (text.toLowerCase()) {
            case "красный" -> Color.RED;
            case "зеленый" -> Color.GREEN;
            case "синий" -> Color.BLUE;
            default -> Color.BLACK; // Цвет по умолчанию
        };
    }

    @FXML
    void OnMouseClicked(MouseEvent event) {
        Shape source = (Shape) event.getSource();

        if (event.getButton() == MouseButton.PRIMARY) {
            System.out.println(CIRCLE.getFill());

            CIRCLE.setFill(generateRandomColor(random));
        } else if (event.getButton() == MouseButton.SECONDARY) {
            CIRCLE.setFill(Paint.valueOf("radial-gradient(focus-angle 0.0deg, focus-distance 0.0% , center 50.0% 50.0%, radius 50.0%, 0xf2ff00ff 0.0%, 0xc90a0aff 100.0%)"));
        }
    }

    public void setCIRCLE(Circle circle) {
        this.CIRCLE = circle;
    }

    public void setLINE(Line line) {
        this.LINE = line;
    }

    public void setRECTANGLE(Rectangle rectangle) {
        this.RECTANGLE = rectangle;
    }

    @FXML
    void actionMenuCIRCLE(ActionEvent event) {
        figura.setText("круг");
    }

    @FXML
    void actionMenuLINE(ActionEvent event) {
        figura.setText("линия");
    }

    @FXML
    void actionMenuRECTANGLE(ActionEvent event) {
        figura.setText("прямоугольник");
    }

    @FXML
    void actionRed(ActionEvent event) {
        svet.setText("красный");
        color1 = Color.RED;
    }

    @FXML
    void actionGreen(ActionEvent event) {
        svet.setText("зеленый");
        color1 = Color.GREEN;
    }

    @FXML
    void actionBlue(ActionEvent event) {
        svet.setText("синий");
        color1 = Color.BLUE;
    }


    @FXML
    void panedragged(MouseEvent event) {
        endMouseX = event.getX();
        endMouseY = event.getY();
        double currentWidth = SLIDER.getValue();
        switch (figura.getText()) {
            case "круг":
                double radius = Math.sqrt(Math.pow(startMouseX - endMouseX, 2) + Math.pow(startMouseY - endMouseY, 2));
                CIRCLE.setRadius(radius);
                break;

            case "линия":
                LINE.setEndX(endMouseX);
                LINE.setEndY(endMouseY);
                break;

            case "прямоугольник":
                RECTANGLE.setWidth(Math.abs(startMouseX - endMouseX));
                RECTANGLE.setHeight(Math.abs(startMouseY - endMouseY));
                RECTANGLE.setX(Math.min(startMouseX, endMouseX));
                RECTANGLE.setY(Math.min(startMouseY, endMouseY));
                break;
        }

    }

    @FXML
    void panepressed(MouseEvent event) {
        color1 = determineColor(svet.getText());
        startMouseX = event.getX();
        startMouseY = event.getY();
        ShapeProduct product = ShapeFactory.createShape(figura.getText());

        if (product != null) {
            product.setup(pane1, startMouseX, startMouseY, color1, SLIDER.getValue(), this);
        }
    }
    @FXML
    void OnScroll(ScrollEvent event) {
        if (event.getSource() instanceof javafx.scene.Node node) {
            double factor = event.getDeltaY() > 0 ? 1.05 : 0.95;
            if (node instanceof javafx.scene.shape.Line line) {
                line.setStrokeWidth(line.getStrokeWidth() * factor);
            } else {
                node.setScaleX(node.getScaleX() * factor);
                node.setScaleY(node.getScaleY() * factor);
            }
        }
    }

    @FXML
    void OnMouseExited(MouseEvent event) {
        RECTANGLE.setFill(generateRandomColor(random));

    }


    @FXML
    void MouseMoved(MouseEvent event) {
        RECTANGLE.setFill(Paint.valueOf("linear-gradient(from 93.3649% 0.0% to 70.6161% 100.0%, #a15e51 0.0%, #18f500 100.0%)"));

    }
    @FXML
    void LineMouseDragged(MouseEvent event) {
        double deltaX = event.getSceneX() - anchorX;
        double deltaY = event.getSceneY() - anchorY;
        double newAngle = Math.atan2(deltaY, deltaX) * 180 / Math.PI + 90;

        LINE.setRotate(startAngle + newAngle);

    }

    @FXML
    void LineMousePressed(MouseEvent event) {
        anchorX = event.getSceneX();
        anchorY = event.getSceneY();
        startAngle = LINE.getRotate();
    }

    //3 задание
    @FXML
    void actionpressedPane3(MouseEvent event) {
        if (event.getButton() == MouseButton.PRIMARY)
            mouseOldX = event.getSceneX();
        mouseOldY = event.getSceneY();
        System.out.println("press");
    }

    @FXML
    void pane3Dragged(MouseEvent event) {
        if (event.isPrimaryButtonDown()) {
            mouseDeltaX = event.getSceneX() - mouseOldX;
            mouseDeltaY = event.getSceneY() - mouseOldY;
            rotateXAsis.setAngle(rotateXAsis.getAngle() - mouseDeltaY * mouseSensitivity);
            rotateYAsis.setAngle(rotateYAsis.getAngle() + mouseDeltaX * mouseSensitivity);
            mouseOldX = event.getSceneX();
            mouseOldY = event.getSceneY();
            System.out.println("dragged");


        }
    }

    @FXML
    void scrollPane3(ScrollEvent event) {
        double delta = event.getDeltaY();
        if (delta > 0) {
            translate.setZ(translate.getZ() + movementSpeed);
        } else {
            translate.setZ(translate.getZ() - movementSpeed);
        }
        System.out.println("scroll");
    }
    @FXML
    void paneclick(MouseEvent event) {

    }

    public void box() {
        material.setDiffuseColor(Color.ORANGE);
        box.setMaterial(material);

        rotateXAsis = new Rotate(0, Rotate.X_AXIS);
        rotateYAsis = new Rotate(0, Rotate.Y_AXIS);
        translate = new Translate();
        box.getTransforms().setAll(rotateXAsis, rotateYAsis, translate);
        AmbientLight ambientLight = new AmbientLight(Color.rgb(100, 100, 100));

        PointLight pointLight = new PointLight(Color.WHITE);
        pointLight.setTranslateX(200);
        pointLight.setTranslateY(-200);
        pointLight.setTranslateZ(-400);
        modelGroup.getChildren().clear();
        modelGroup.getChildren().addAll(box, ambientLight, pointLight);
        if (!pane3.getChildren().contains(modelGroup)) {
            pane3.getChildren().add(modelGroup);
        }

        modelGroup.setTranslateX(400);
        modelGroup.setTranslateY(300);
    }
    @FXML
    void initialize() {
        box();

    }
}

lineproduct: 
package com.example.pr2;

import javafx.scene.layout.AnchorPane;
import javafx.scene.paint.Paint;
import javafx.scene.shape.Line;

public class LineProduct implements ShapeProduct {
    @Override
    public void setup(AnchorPane pane, double x, double y, Paint color, double strokeWidth, HelloController controller) {
        Line line = new Line(x, y, x + 1, y + 1);
        line.setStroke(color);
        line.setStrokeWidth(strokeWidth);
        line.setOnMousePressed(controller::LineMousePressed);
        line.setOnMouseDragged(controller::LineMouseDragged);
        line.setOnScroll(controller::OnScroll);
        pane.getChildren().add(line);
        controller.setLINE(line);
    }
}

rectangleproduct:
package com.example.pr2;

import javafx.scene.layout.AnchorPane;
import javafx.scene.paint.Paint;
import javafx.scene.shape.Rectangle;
import javafx.scene.paint.Color;

public class RectangleProduct implements ShapeProduct {
    @Override
    public void setup(AnchorPane pane, double x, double y, Paint color, double strokeWidth, HelloController controller) {
        Rectangle rect = new Rectangle(x, y, 20, 20);
        rect.setFill(color);
        rect.setStroke(Color.BLACK);
        rect.setStrokeWidth(strokeWidth);
        rect.setOnScroll(controller::OnScroll);
        rect.setOnMouseMoved(controller::MouseMoved);
        rect.setOnMouseExited(controller::OnMouseExited);
        pane.getChildren().add(rect);
        controller.setRECTANGLE(rect);
    }
}

shapefactory: 
package com.example.pr2;

public class ShapeFactory {
    public static ShapeProduct createShape(String type) {
        return switch (type.toLowerCase()) {
            case "круг" -> new CircleProduct();
            case "линия" -> new LineProduct();
            case "прямоугольник" -> new RectangleProduct();
            default -> null;
        };
    }
}

shapeproduct: 
package com.example.pr2;

import javafx.scene.layout.AnchorPane;
import javafx.scene.paint.Paint;

public interface ShapeProduct {
    // 1: панель, 2-3: координаты, 4: цвет, 5: толщина, 6: контроллер для событий
    void setup(AnchorPane pane, double x, double y, Paint color, double strokeWidth, HelloController controller);
}
